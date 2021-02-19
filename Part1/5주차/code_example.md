# Code Example(학점계산기)
우선, 필요한 클래스를 생각해본다. **학생, 점수, 과목** 을 기본적으로 생각할 수 있다.
기본설정은 학교를 다니는 학생들은 필수과목이 정해져있고, 필수과목과 일반과목의 학점 선정 방식이 다르다. 그 방식에 따른 학점을 계산한다.

+ 학생과 점수의 관계 : 학생은 여러 과목의 점수를 가진다(has-A)
+ 학생과 과목의 관계 : 한 학생은 여러 과목을 수강한다. **한 과목엔 여러 수강생이 있다.** 이번 코드에선 후자 선택.
+ 과목과 점수의 관계 : 한 학생의 어느 과목의 점수가 몇 점이다. (일 대 일 구조)

Student.java

	package school;

	import java.util.ArrayList;

	public class Student {
		private int studentId;
    	private String studentName;
    	private Subject majorSubject;	//필수 과목

	    private ArrayList<Score> scoreList = new ArrayList<Score>();
	
    	public Student( int studentId, String studentName, Subject majorSubject){
    	    this.studentId = studentId;
    	    this.studentName = studentName;
    	    this.majorSubject = majorSubject;
    	}
	
    	public void addSubjectScore(Score score){
    	    scoreList.add(score);
    	}
	
    	... //getter와 setter(여기엔 setter가 다 적혀있지만 캡슐화 위해 setter를 없애는 쪽으로 짜는 것이 바람직하다.)
	}		

Score.java

	package school;

	public class Score {
	  	int studentId;
    	Subject subject;
    	int point;
	
    	public Score( int studentId, Subject subject, int point){
    	    this.studentId = studentId;
    	    this.subject = subject;
    	    this.point = point;
    	}
	
		...//getter setter
	
    	public String toString(){
    	    return "학번:" + studentId + "," + subject.getSubjectName() + ":" + point;
    	}
	}

Subject.java

	package school;

	import java.util.ArrayList;
	import utils.Define;


	public class Subject {
		private String subjectName;
	    private int subjectId;
    	private int gradeType;
	
	
    	private ArrayList<Student> studentList = new ArrayList<Student>();
	
    	public Subject(String subjectName, int subjectId){
    	    this.subjectName = subjectName;
    	    this.subjectId = subjectId;
    	    this.gradeType = Define.AB_TYPE;
    	}
	
    	... // getter setter
	
    	public void register(Student student){  
    	    studentList.add(student);
    	}
	}

상수 값 정의하는 Define 클래스 있다. 국어와 수학에 대한 숫자 과목코드? 정하고, 일반과목과 필수과목 학점 산출 정책을 0, 1로 설정해서 학점산출방식코드 설정한다.

	package utils;

	public class Define {

    	public static final int KOREAN = 1001;
    	public static final int MATH = 2001;

    	public static final int AB_TYPE = 0;
    	public static final int SAB_TYPE = 1; 
    	public static final int PF_TYPE = 2;
	}

**학점 산정 방식** 을 정하는 클래스를 정해보자. 인터페이스에 학점 선정 메서드를 선언하고 그 밑에 구현하는 클래스에서 메서드를 오버라이딩해서 학점 선정 방식만 바꿔준다.


GradeEvaluation 인터페이스

	package grade;

	public interface GradeEvaluation {
		public String getGrade(int point);
	}

BasicEvaluation.java

	package grade;

	public class BasicEvaluation implements GradeEvaluation{

	    @Override
	    public String getGrade(int point) {
	        String grade;
	
	        if(point >=90 && point <=100)
	            grade = "A";
	        else if(point >=80 && point <=89)
	            grade = "B";
	        else if(point >=70 && point <=79)
	            grade = "C";
	        else if(point >=55 && point <=69)
	            grade = "D";
	        else
	            grade = "F";
	        return grade;
	    }
	}

MajorEvaluation.java

	package grade;

	public class MajorEvaluation implements GradeEvaluation {

	    @Override
    	public String getGrade(int point) {
    	    String grade;
    	    if(point >=95 && point <=100)
    	        grade = "S";
    	    else if(point >=90 && point <=94)
    	        grade = new String("A");
    	    else if(point >=80 && point <=89)
    	        grade = "B";
    	    else if(point >=70 && point <=79)
    	        grade = "C";
    	    else if(point >=60 && point <=69)
    	        grade = "D";
    	    else
    	        grade = "F";
    	    return grade;
    	}
	}

이제 **리포트 클래스** 를 짜보자. 르포트 클래스는 실제 서비스를 운영하는 쇼핑몰이나 금융, 검색 등에서 리포트 프로그램이 많은 양을 차지한다. (데이터 베이스를 사용하는 경우에는 데이터 베이스로부터 값을 가져온느 쿼리문을 최적화하여 구현한다.)

GenerateGradeReport.java

	package school.report;

	import java.util.ArrayList;

	import grade.BasicEvaluation;
	import grade.GradeEvaluation;
	import grade.MajorEvaluation;
	import school.School;
	import school.Score;
	import school.Student;
	import school.Subject;
	import utils.Define;

	public class GenerateGradeReport {

	    School school = School.getInstance();
    	public static final String TITLE = " 수강생 학점 \t\t\n";
    	public static final String HEADER = " 이름  |  학번  |필수과목| 점수   \n ";
    	public static final String LINE = "-------------------------------------\n";
    	private StringBuffer buffer = new StringBuffer();
	
    	public String getReport(){
    	    ArrayList<Subject> subjectList = school.getSubjectList();
    	    for( Subject subject : subjectList) {
    	        makeHeader(subject);
    	        makeBody(subject);
    	        makeFooter();
    	    }
    	    return buffer.toString();
    	}
	
    	public void makeHeader(Subject subject){
    	    buffer.append(GenerateGradeReport.LINE);
    	    buffer.append("\t" + subject.getSubjectName());
    	    buffer.append(GenerateGradeReport.TITLE );
    	    buffer.append(GenerateGradeReport.HEADER );
    	    buffer.append(GenerateGradeReport.LINE);
    	}
	
    	public void makeBody(Subject subject){
	
    	    ArrayList<Student> studentList = subject.getStudentList();
	
    	    for(int i=0; i<studentList.size(); i++){
    	        Student student = studentList.get(i);
    	        buffer.append(student.getStudentName());
    	        buffer.append(" | ");
    	        buffer.append(student.getStudentId());
    	        buffer.append(" | ");
    	        buffer.append(student.getMajorSubject().getSubjectName() + "\t");
    	        buffer.append(" | ");
	
    	        getScoreGrade(student, subject.getSubjectId());
    	        buffer.append("\n");
    	        buffer.append(LINE);
    	    }
    	}
	
    	public void getScoreGrade(Student student, int subjectId){
	
    	    ArrayList<Score> scoreList = student.getScoreList();
    	    int majorId = student.getMajorSubject().getSubjectId();
	
    	    GradeEvaluation[] gradeEvaluation = {new BasicEvaluation(), new MajorEvaluation()};
	
    	    for(int i=0; i<scoreList.size(); i++){
	
    	        Score score = scoreList.get(i);
    	        if(score.getSubject().getSubjectId() == subjectId) {
    	            String grade;
    	            if(score.getSubject().getSubjectId() == majorId)
    	                grade = gradeEvaluation[Define.SAB_TYPE].getGrade(score.getPoint());
    	            else
    	                grade = gradeEvaluation[Define.AB_TYPE].getGrade(score.getPoint());
    	            buffer.append(score.getPoint());
    	            buffer.append(":");
    	            buffer.append(grade);
    	            buffer.append(" | ");
    	        }
    	    }
    	}
	
    	public void makeFooter(){
    	    buffer.append("\n");
    	}
	}	
	

**학교 클래스** 를 짜보자 학교 클래스의 속성은 학생 리스트(학생은 점수 리스트 포함), 과목 리스트가 있다. 학교는 유일한 객체이므로 싱글톤 패턴으로 구현한다.

School.java

	package school;

	import java.util.ArrayList;

	public class School {

	    private static School instance = new School();
	
	    private static String SCHOOL_NAME = "Good School";
	    private ArrayList<Student> studentList = new ArrayList<Student>();
	    private ArrayList<Subject> subjectList = new ArrayList<Subject>();
	
	    private School(){}
	
	    public static School getInstance(){
	        if(instance == null)
	            instance = new School();
	        return instance;
	    }
	
	    public ArrayList<Student> getStudentList(){
	        return studentList;
	    }
	
	    public void addStudent(Student student){
	        studentList.add(student);
	    }
	
	    public void addSubject(Subject subject) {
	        subjectList.add(subject);
	    }
	
	    public ArrayList<Subject> getSubjectList() {
	        return subjectList;
	    }
	
	    public void setSubjectList(ArrayList<Subject> subjectList) {
	        this.subjectList = subjectList;
	    }
	}

학생, 과목, 점수 등을 각각 생성하고 리포트 클래스를 생성해서 성적과 학점을 출력하는 **테스트 클래스** 를 만들어보자.

TestMain.java

	package test;

	import school.School;
	import school.Score;
	import school.Student;
	import school.Subject;
	import school.report.GenerateGradeReport;
	import utils.Define;

	public class TestMain {

	    School goodSchool = School.getInstance();
    	Subject korean;
    	Subject math;
	
    	GenerateGradeReport gradeReport = new GenerateGradeReport();
	
    	public static void main(String[] args) {
	
    	    TestMain test = new TestMain();
	
    	    test.creatSubject();
    	    test.createStudent();
	
    	    String report = test.gradeReport.getReport();
    	    System.out.println(report);
	
    	}
	
    	public void creatSubject(){
	
    	    korean = new Subject("국어", Define.KOREAN);
    	    math = new Subject("수학", Define.MATH);
	
    	    goodSchool.addSubject(korean);
    	    goodSchool.addSubject(math);
    	}
	
    	public void createStudent(){
	
    	    Student student1 = new Student(181213, "안성원", korean  );
    	    Student student2 = new Student(181518, "오태훈", math  );
    	    Student student3 = new Student(171230, "이동호", korean  );
    	    Student student4 = new Student(171255, "조성욱", korean  );
    	    Student student5 = new Student(171590, "최태평", math );
	
    	    goodSchool.addStudent(student1);
    	    goodSchool.addStudent(student2);
    	    goodSchool.addStudent(student3);
    	    goodSchool.addStudent(student4);
    	    goodSchool.addStudent(student5);
		
    	    korean.register(student1);
    	    korean.register(student2);
    	    korean.register(student3);
    	    korean.register(student4);
    	    korean.register(student5);
	
    	    math.register(student1);
    	    math.register(student2);
    	    math.register(student3);
    	    math.register(student4);
    	    math.register(student5);
	
    	    addScoreForStudent(student1, korean, 95);
    	    addScoreForStudent(student1, math, 56);
	
    	    addScoreForStudent(student2, korean, 95);
    	    addScoreForStudent(student2, math, 95);
	
    	    addScoreForStudent(student3, korean, 100);
    	    addScoreForStudent(student3, math, 88);
	
    	    addScoreForStudent(student4, korean, 89);
    	    addScoreForStudent(student4, math, 95);
	
    	    addScoreForStudent(student5, korean, 85);
    	    addScoreForStudent(student5, math, 56);
    	}

    		public void addScoreForStudent(Student student, Subject subject, int point){
	
    	    Score score = new Score(student.getStudentId(), subject, point);
    	    student.addSubjectScore(score);
	
    	}
	}	
	
