#디자인 패턴

>옵서버패턴(Observer pattern)

데이터의 변경이 발생했을 경우 상대 클래스나 객체의 의존하지 않으면서 데이터 변경을 통보하고자 할 때 유용하다. 
통보 대상 객체의 관리를 subject 클래스와 Observer 인터페이스로 일반화 한다. 
그러면 데이터 변경을 통보하는 클래스는 통보 대상 클래스나 객체에 대한 의존성을 없앨 수 있다.
결과적으로 옵서버 패턴은 통보 대상 클래스나 대상 객체의 변경에도 ConcreteSubject 클래스를 수정 없이 그대로 사용할 수 있도록 한다.

Observer : 데이터의 변경을 통보 받는 인터페이스. 즉 subject 에서는 Observer 인터페이스의 update 메서드를 호출함으로써 ConcreteSubject 의 데이터 변경을 ConcreteObserver 에게 통보한다.

Subject : ConcreteObserver 객체를 관리하는 요소. Observer 인터페이스를 참조해서 ConcreteObserver 를 관리하므로 ConcreteObserver 변화에 독립적일 수 있다.

ConcreteSubject : 변경 관리 대상이 되는 데이터가 있는 클래스. 데이터 변경을 위한 메서드인 setState 가 있으며 setState 에서는 자신의 데이터인 subjectState 를 변경하고 subject 의 notifyObserver 메서드를 호출해서 ConcreteObserver 객체에 변경을 통보한다.

ConcreteObserver : ConcreteSubject 의 변경을 통보 받는 클래스. Observer 인터페이스의 update 메서드를 구현함으로써 변경을 통보받는다. 변경된 데이터는 ConcreteSubject 의 getState 메서드를 호출함으로써 변경을 조회한다.

~~~

public interface Observer { // 추상화 된 통보 대상
    public abstract void update();  //데이터의 변경을 통보했을 때 처리하는 메서드
}

public abstract class Subject { //추상화된 변경 관심 대상 데이터
    private List<Observer> observers = new ArrayList();  //추상화된 통보 대상 목록
    
    public void attach(Observer observer){  //옵서버 통보 대상을 추가함
        observers.add(observer);
    }
    
    public void detach(Observer observer){  //옵서버 통보 대상을 제거함
        observers.remove(observer);
    }
    
    public void notifyObservers(){  // 통보 대상 목록 각 옵서버에게 변경을 통보함
        for(Observer o : observers){
            o.update();
        }
    }

}

public class ScoreRecord extends Subject {  //구체적인 변경 감시 대상 데이터
    private List<Integer> scores = new ArrayList();
    
    public void addScore(int score){
        scores.add(score);
        
        //데이터가 변경되면 각 옵서버에게 변경을 통보함 
        notifyObservers();
    }
    
    public List<Integer> getScoreRecord(){
        return scores;
    }
}

public class DataSeetView implements Observer {
    private ScoreRecord scoreRecord;
    private int viewCount;
    
    public DataSeetView(ScoreRecord scoreRecord, int viewCount){
        this.scoreRecord = scoreRecord;
        this.viewCount = viewCount;
    }
    
    public void update(){   //점수의 변경을 통보 받음
        List<Integer> record = scoreRecord.getScoreRecord();    //점수 조회
        displayScores(record, viewCount);   //조회된 점수를 viewCount 만큼만 출력함
    }
    
    private void displayScores(List<Integer> record, int viewCount){
        System.out.println("List of : " + viewCount + "entries: ");
        for(Integer score: record){
            System.out.print(score + ",");
        }
    }

}

public class MinMaxView implements Observer {
    private ScoreRecord scoreRecord;
    
    public MinMaxView(ScoreRecord scoreRecord){
        this.scoreRecord = scoreRecord;
    }
    
    public void update(){   //점수의 변경을 통보 받음
        List<Integer> record = scoreRecord.getScoreRecord();    //점수 조회
        displayMinMaxView(record);   //최소, 최대값 출력
    }
    
    private displayMinMaxView(List<Integer> record){
        int min = Collections.min(record, null);
        int max = Collections.max(record, null);
        System.out.println("최소값 : " + min + " 최대값: " + max);
    }
}

public class StatisticsView implements Observer {   //Observer를 구현함으로써 통보 대상이 됨
    private ScoreRecord scoreRecord;
    
    public StatisticsView(ScoreRecord scoreRecord){
        this.scoreRecord = scoreRecord;
    }
    
    public void update(){
        List<Integer> record = scoreRecord.getScoreRecord();
        
        displayStatistics();    //  변경 통보 시 조회된 점수의 함과 평균을 출력함 
    }
    
    private void displayStatistics(List<Integer> record){   //합과 평균을 출력
        int sum = 0;
        for(int score : record){
            sum += score;
        }
        
        float average = (float) sum / record.size();
        System.out.println("Sum : " + sum + " Average : " + average);
    }

}

public class Client {
    public static void main(String[] args){
        ScoreRecord scoreRecord = new ScoreRecord();
        
        DataSeetView dataSeetView = new DataSeetView(scoreRecord, 3);
        scoreRecord.attach(dataSeetView);
        
        MinMaxView minMaxView = new MinMaxView(scoreRecord);
        scoreRecord.attach(minMaxView);
        
        //3개의 목록 DataSeetView, 5개 목록 DataSeetView 그리고 MinMaxView 가 Observer 로 설정됨 
        for(int index = 1; index <= 5; index++){
            int score = index * 10;
            System.out.println("Adding : " + score);
            scoreRecord.addScore(score);    // 각 점수 추가시 최대 3개 목록, 5개 목록, 최소/최대 값 출력
        }
        
        
        scoreRecord.detach(dataSeetView); //3개 목록 DataSeetView는 이제 Observer가 아님
        
        StatisticsView statisticsView = new StatisticsView(scoreRecord);
        scoreRecord.attach(statisticsView); //StatisticsView 가 Observer로서 설정됨
        
        //5개의 목록 DataSeetView, 5개 목록 DataSeetView, MinMaxView 그리고 StatisticsView 가 Observer 로 설정됨 
        for(int index = 1; index <= 5; index++){
            int score = index * 10;
            System.out.println("Adding : " + score);
            scoreRecord.addScore(score);    // 각 점수 추가시 최대 5개 목록, 최소/최대 값 출력, 합/ 평균을 출력
        }
        
    }
}


~~~

