MOCHA란
Mocha는 Node.js와 브라우저에서 실행되는 JavaScript 테스트 프레임 워크이다. 비동기 테스트를 만들 수 있다. 자체적으로 assertion을 지원하지 않기 때문에 다른 Assertion라이브러리와 함께 사용해야 한다.

자바스크립트로 테스트 종류
Karma
Jasmine
Jest
Chai
Mocha
공식문서 : https://mochajs.org/



문법
describe - 테스트할 그룹

before - 테스트할 그룹에서 시작 전에 실행

beforeEach - 테스트할 그룹에서 각각의 테스트 시작 전에 실행

afterEach - 테스트할 그룹에서 각각의 테스트 종료 후에 실행

after - 테스트할 그룹에서 종료 후에 실행

it- 테스트 케이스



준비
node.js, npm, mocha
mocha 설치 :npm -g install mocha


버전확인
npm -v
node -v
mocha -V

function add(x, y) {

    return x + y;
}
function moreThan3(x) {

    return x >= 3;
}
describe('테스트', () =>{

    before(() => {
        console.log('---------------- 잔체 테스트 시작 전 -------------- ');
    });

    after(() => {
        console.log('---------------- 전체 테스트 끝 -------------- ');
    });

    beforeEach(() => {
        console.log('**테스트 시작 **');
    });
    afterEach(() => {
        console.log('**테스트 종료**');
    });


    it("덧셈", () =>{
        assert.equal(3, add(1,2));
        assert.equal(10, add(5,5));
    });
    it("3보다 큰지 확인", () => {
        assert.equal(true, moreThan3(5));
        assert.equal(false, moreThan3(1));

    });
});

