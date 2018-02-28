# Hibernate-Denemeleri
Bu dökümanda Hibernate ile ilgili denemelerim bulunmaktadır.


# OneToOne 

----Laptop.java

@Entity

public class Laptop {
	
	@Id
	private int lId;
	private String lName;
	
	public int getlId() {
		return lId;
	}
	public void setlId(int lId) {
		this.lId = lId;
	}
	public String getlName() {
		return lName;
	}
	public void setlName(String lName) {
		this.lName = lName;
	}
}



----Student.java

@Entity

public class Student {
	
	@Id
	private int rolNo;
	private String sName;
	private int marks;
	
	@OneToOne
	private Laptop laptop;
  
	public int getRolNo() {
		return rolNo;
	}
	public void setRolNo(int rolNo) {
		this.rolNo = rolNo;
	}
	public String getsName() {
		return sName;
	}
	public void setsName(String sName) {
		this.sName = sName;
	}
	public int getMarks() {
		return marks;
	}
	public void setMarks(int marks) {
		this.marks = marks;
	}		
	public Laptop getLaptop() {
		return laptop;
	}
	public void setLaptop(Laptop laptop) {
		this.laptop = laptop;
	}
	@Override
	public String toString()
	{
	return "Student [rolNo="+rolNo+", sName="+sName+", marks= "+marks+" ]";	
	}
}




----App.java

public class App 
{


    public static void main( String[] args )
    {
        Laptop laptop = new Laptop();
        laptop.setlId(102);
        laptop.setlName("Mac");
        
        Student student=new Student();
        student.setRolNo(1012);
        student.setsName("Ruddyy");
        student.setMarks(20);
        
        //One To One        
          student.setLaptop(laptop);
        
        Configuration con =new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Laptop.class).addAnnotatedClass(Student.class);
        ServiceRegistry reg =new ServiceRegistryBuilder().applySettings(con.getProperties()).buildServiceRegistry();       
        SessionFactory sf= con.buildSessionFactory(reg);  
        Session session=sf.openSession();
        
        session.beginTransaction();
        
        session.save(laptop);
        session.save(student);
        
        session.getTransaction().commit();
        
    }
}



----hibernate.cfg.xml

<hibernate-configuration>
    <session-factory>        
        <property name="hbm2ddl.auto">update</property>
        <property name="show_sql">true</property>        
    </session-factory>
</hibernate-configuration>

******************************************************************************************




Hibernate: insert into Laptop (lName, lId) values (?, ?)

Hibernate: insert into Student (laptop_lId, marks, sName, rolNo) values (?, ?, ?, ?)




*******************************************************************************************

# OneToMany

----Laptop.java

@Entity

public class Laptop {
	
	@Id
	private int lId;
	private String lName;
	
	@ManyToOne
	private Student student;
	
	public int getlId() {
		return lId;
	}
	public void setlId(int lId) {
		this.lId = lId;
	}
	public String getlName() {
		return lName;
	}
	public void setlName(String lName) {
		this.lName = lName;
	}
	public Student getStudent() {
		return student;
	}
	public void setStudent(Student student) {
		this.student = student;
	}

}




----Student.java

@Entity

public class Student {
	
	@Id
	private int rolNo;
	private String sName;
	private int marks;
	
	/*
	@OneToOne
	private Laptop laptop;
	*/
	
	@OneToMany(mappedBy="student")
	private List<Laptop> laptop=new ArrayList<Laptop>();
	
	public int getRolNo() {
		return rolNo;
	}
	public void setRolNo(int rolNo) {
		this.rolNo = rolNo;
	}
	public String getsName() {
		return sName;
	}
	public void setsName(String sName) {
		this.sName = sName;
	}
	public int getMarks() {
		return marks;
	}
	public void setMarks(int marks) {
		this.marks = marks;
	}		

	public List<Laptop> getLaptop() {
		return laptop;
	}
	public void setLaptop(List<Laptop> laptop) {
		this.laptop = laptop;
	}
	@Override
	public String toString()
	{
	return "Student [rolNo="+rolNo+", sName="+sName+", marks= "+marks+" ]";	
	}
}



----App.java

public class App 
{

    public static void main( String[] args )
    {
    
        Laptop laptop = new Laptop();
        laptop.setlId(102);
        laptop.setlName("Mac");
        
        Student student=new Student();
        student.setRolNo(1012);
        student.setsName("Ruddyy");
        student.setMarks(20);

        student.getLaptop().add(laptop);
        
        Configuration con =new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Laptop.class).addAnnotatedClass(Student.class);
        ServiceRegistry reg =new ServiceRegistryBuilder().applySettings(con.getProperties()).buildServiceRegistry();       
        SessionFactory sf= con.buildSessionFactory(reg);  
        Session session=sf.openSession();
        
        session.beginTransaction();
        
        session.save(laptop);
        session.save(student);
        
        session.getTransaction().commit();
        
    }
}



hibernate.cfg.xml


<hibernate-configuration>
    <session-factory>        
        <property name="hbm2ddl.auto">update</property>
        <property name="show_sql">true</property>        
    </session-factory>
</hibernate-configuration>

******************************************************************************************************

Hibernate: insert into Laptop (lName, student_rolNo, lId) values (?, ?, ?)

Hibernate: insert into Student (marks, sName, rolNo) values (?, ?, ?)

*******************************************************************************************************


# ManyToMany


----Laptop.java

@Entity

public class Laptop {
	
	@Id
	private int lId;
	private String lName;
	
	@ManyToMany
	private List<Student> student=new ArrayList<Student>();
	
	public int getlId() {
		return lId;
	}
	public void setlId(int lId) {
		this.lId = lId;
	}
	public String getlName() {
		return lName;
	}
	public void setlName(String lName) {
		this.lName = lName;
	}
	public List<Student> getStudent() {
		return student;
	}
	public void setStudent(List<Student> student) {
		this.student = student;
	}
}



----Student.java

@Entity

public class Student {
	
	@Id
	private int rolNo;
	private String sName;
	private int marks;
	
	/*
	@OneToOne
	private Laptop laptop;
	*/
	
	@ManyToMany(mappedBy="student")
	private List<Laptop> laptop=new ArrayList<Laptop>();
	
	public int getRolNo() {
		return rolNo;
	}
	public void setRolNo(int rolNo) {
		this.rolNo = rolNo;
	}
	public String getsName() {
		return sName;
	}
	public void setsName(String sName) {
		this.sName = sName;
	}
	public int getMarks() {
		return marks;
	}
	public void setMarks(int marks) {
		this.marks = marks;
	}		

	public List<Laptop> getLaptop() {
		return laptop;
	}
	public void setLaptop(List<Laptop> laptop) {
		this.laptop = laptop;
	}
	@Override
	public String toString()
	{
	return "Student [rolNo="+rolNo+", sName="+sName+", marks= "+marks+" ]";	
	}
}



----App.java
public class App 
{

    public static void main( String[] args )
    {
    
        Laptop laptop = new Laptop();
        laptop.setlId(102);
        laptop.setlName("Mac");
        
        Student student=new Student();
        student.setRolNo(1012);
        student.setsName("Ruddyy");
        student.setMarks(20);

        student.getLaptop().add(laptop);
        laptop.getStudent().add(student);
        
        Configuration con =new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Laptop.class).addAnnotatedClass(Student.class);
        ServiceRegistry reg =new ServiceRegistryBuilder().applySettings(con.getProperties()).buildServiceRegistry();       
        SessionFactory sf= con.buildSessionFactory(reg);  
        Session session=sf.openSession();
        
        session.beginTransaction();
        
        session.save(laptop);
        session.save(student);
        
        session.getTransaction().commit();
        
    }
}


----hibertnate.cfg.xml 
        <property name="hbm2ddl.auto">update</property>
        <property name="show_sql">true</property>
	
***********************************************************************************************************
Hibernate: insert into Laptop (lName, lId) values (?, ?)

Hibernate: insert into Student (marks, sName, rolNo) values (?, ?, ?)

Hibernate: insert into Laptop_Student (laptop_lId, student_rolNo) values (?, ?)
*************************************************************************************************************
