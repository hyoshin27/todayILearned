#DDD start!

>4. 리포지터리와 모델구현(JPA 중심)

한 테이블에 엔티티와 밸류가 같이 있다면 밸류는 @Embeddable 로 매핑 설정, 밸류 타입 프로퍼티는 @Embedded 로 매핑 설정


X는 밸류타입 Y는 DB 타입일때
 
~~~

public interface AttributeConverter<X,Y> {
    public Y convertToDatabaseColumn (X attribute);
    public X convertToEntityAttribute (Y dbData);
}
~~~

다른방법

~~~
@Converter(autoApply = true)
public class MoneyConverter implements AttributeConverter<Money, Integer>{
    @Override
    public Integer convertToDatabaseColumn(Money money){
        if(Objects.isNull(money)){
            return null;
        }else{
            return money.getValue();
        }
    }
    
    @Override
    public Integer convertToEntityAttribute(Integer value){
        if(Objects.isNull(value)){
            return null;
        }else{
            return new Money(value);
        }
    }
}

@Entity
public class Order {
    private Money totalAmounts; //MoneyConverter를 통해 값 변환
}
~~~

autoApply = true로 하면 모든 Money가 자동 적용 된다. 기본은 false 이며 프로퍼티 값 변환할때 사용할 컨버터를 직접 지정할 수 있다. 


밸류컬렉션을 별도 테이블로 매핑할때는 @ElementCollection 과 @CollectionTable 을 함께 사용한다.

~~~
@Entity
@Table(name = "purchase_order")
public class Order {
    @ElementCollection
    @CollectionTable(name = "order_line", joinColumn = @JoinColumn(name = "order_number"))
    @OrderColumn(name = "line_idx")
    private List<OrderLine> orderLines;
}

@Embeddable
public class OrderLine {
    @Embedded
    private ProductId productId;
    
    @Column(name = "price")
    private Money price;
    
    @Column(name = "quantity")
    private Money quantity;
    
    @Column(name = "amounts")
    private Money amounts;
}
~~~

-orderLine 의 매핑을 함께 표시했는데 orderLine 에는 list 의 인덱스 값을 저장하기 위한 프로퍼티가 존재하지 않는다. 그 이유는 list 타입 자체가 인덱스를 갖고 있기 때문

-jpa 는 @OrderColumn 을 이용해서 지정한 칼럼에 리스트의 인덱스 값을 저장한다.

-@CollectionTable 은 밸류를 저장할 테이블을 지정할 때 사용

-name 속성으로 테이블 이름을 지정하고 joinColumns 속성은 외부키로 사용하는 칼럼을 지정, 외부키가 2개 이상인 경우 @JoinColumn의 배열을 이용해서 외부키 목록을 지정한다.

밸류컬렉션

-별도의 테이블이 아닌 한개의 칼럼에 저장해야 할때

~~~
public class EmailSet {
    private Set<Email> emails = new HashSet<>();
    
    private EmailSet(){}
    private EmailSet(Set<Email> emails){
        this.emails.addAll(emails);
    }
    
    public Set<Email> getEmails(){
        return Collections.unmodifiableSet(emails);
    }

}
~~~

AttributeConverter

~~~
@Converter
public class EmailSetConverter implements AttributeConverter<EmailSet, String> {
    
    @Override
    public String convertToDatabaseColumn(EmailSet attribute){
        if(Objects.isNull(attribute)){
            return null;
        }
        return attribute.getEmails()
                        .stream()
                        .map(Email::toString)
                        .collect(Collectors.Joining(","));
    }
    
     @Override
     public EmailSet convertToEntityAttribute(String dbData){
        if(Objects.isNull(dbData)){
            return null;
        }
        String[] emails = dbData.split(",");
        Set<Email> emailSet = Arrays.stream(emails)
                                    .map(value -> new Email(value))
                                    .collect(toSet());
     
     }
    
}
~~~

converter 로 EmailSetConverter 를 사용하기

~~~
@Column(name = "emails")
@Convert(converter = EmailSetConverter.class)
private EmailSet emailSet;

~~~

조회시점에서 애그리거트가 완전한 상태가 되도록 하려면 즉시 로딩(FetchType.EAGER)


애그리거트가 완전해야 하는 이유

- 상태를 변경하는 기능을 실행할때 애그리거트 상태가 완전해야 한다.

- 표현 영역에서 상태 정보를 보여줄 때 필요하다.


