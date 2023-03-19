# EQUINOX-HACKATHON
import sys
import mysql.connector as mys

con=mys.connect(host="localhost",user="root",password="demo")

mycur=con.cursor()
db="Student_Portal"

sql="CREATE DATABASE if not exists {}".format(db)               # parameterised query
mycur.execute(sql)
print("Database created successfully!!!")

mycur=con.cursor()
mycur.execute("Use "+db)

tablename="student_record1"
query="Create table if not exists "+tablename+" \
(enrollmentno char(10) primary key,\
name char(25) not null,\
date_of_birth char(15),\
gender char(10),\
emailid varchar(30),\
mobileno char(10),\
course varchar(15) not null,\
semester int)"
mycur.execute(query)
print("Table "+tablename+" created successfully!!!")

tablename="course_record"
query="Create table if not exists "+tablename+" \
(semester INT(1),\
branch varchar(5),\
course1 varchar(30),\
course2 varchar(30),\
course3 varchar(30),\
course4 varchar(30),\
course5 varchar(30),\
course6 varchar(30))"
mycur.execute(query)
print("Table "+tablename+" created successfully!!!")

tablename3="exam_record"
query="Create table if not exists "+tablename3+" \
(enrollmentno char(10),\
semester INT(1),\
branch varchar(5),\
mark_sub1 INT,\
mark_sub2 INT,\
mark_sub3 INT,\
mark_sub4 INT,\
mark_sub5 INT,\
mark_sub6 INT,\
total_marks INT,\
Percentage INT,\
Grade char(4))"
mycur.execute(query)
print("Table "+tablename3+" created successfully!!!")

def createrec():     #create record of a particular student
    try:
        rno=int(input("ENROLLMENT NO.:"))
        name=input("STUDENT'S NAME:")
        dob=input("DOB(DD/MM/YYYY):")
        gen=input("GENDER(M/F):")
        cont=input("CONTACT NO.:")
        email=input("EMAIL ID:")
        course=input("COURSE :")
        sem=int(input("SEMESTER(in integer):"))
        rec=[rno,name,dob,gen,email,cont,course,sem]

        query="insert into student_record1 values(%s,%s,%s,%s,%s,%s,%s,%s)"
        mycur.execute(query,rec)        # parameterised query
        con.commit()
        print("Record added successfully!!!")
    except Exception as e:
        print("Something went wrong",e)

def display_student_record():     #Display records of a particular employee
    try:
        query="select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        e=input("Enter enrollment no you want to search")
        found=0
        
        for r in row:
            if e==r[0]:
                found=1
                print(" ")
                print("|"+"-"*50+"|")
                print("|\t\t PERSONAL DETAILS")
                print("|"+"-"*50+"|")
                print("|\tNAME          :   ",r[1],"\t\t")
                print("|\tENROLLMENT NO :   ",r[0],"\n")
                print("|\tDATE_OF_BIRTH :   ",r[2],"\n")
                print("|\tGENDER        :   ",r[3],"\n")
                print("|"+"-"*50+"|")
                print("|\t\t CONTACT DETAILS")
                print("|"+"-"*50+"|")
                print("|\tMOBILE NUMBER :   ",r[5],"\n")
                print("|\tEMAIL ID      :   ",r[4],"\n")
                print("|"+"-"*50+"|")
                print("|\t\t PROGRAMME DETAILS")
                print("|"+"-"*50+"|")
                print("|\tCOURSE        :   ",r[6],"\n")
                print("|\tSEMESTER      :   ",r[7],"\n")

        print("|"+"-"*50+"|")
        print(" ")
        print(" ")
        
        if found==0:
            print("student record not found!!!")
            
    except Exception as e:
        print("something went wrong!!!",e)


def display_records():       #Display all the records present in the table
    try:
        query="select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        
        print(" ")
        print("|"+"-"*128+"|")
        
        print("| ENROLLMENT NO |     NAME      |      DOB     | GENDER  |     EMAIL ID          |    MOBILE NO   |\
    COURSE    |   SEMESTER    |")
        
        print("|"+"-"*128+"|")
        
        for r in row:
            p=9-len(r[0])
            x=9-len(r[1])
            y=11-len(str(r[2]))
            z=8-len(r[3])
            w=10-len(str(r[4]))
            a=15-len(str(r[5]))
            b=11-len(str(r[6]))
            c=4-len(str(r[7]))
            
            print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*"  ","|",\
                  r[5],a*" ","|",r[6],b*" ","|",r[7],"\t\t","|")
            
        print("|"+"-"*128+"|")
        print(" ")
        print(" ")
        
    except Exception as e:
        print("something went wrong!!!",e)
    
def delete_student():     #Deletes record of a particular employee
    try:
        eno=input("Enter enrollment no. you want to delete")
        query="Select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        
        for r in row:
            if eno==r[0]:
                found=1
                print(" ")
                print("Student record to be deleted:")
                print(" ")
                print("|"+"-"*130+"|")
        
                print("| ENROLLMENT NO |     NAME      |      DOB     | GENDER  |     EMAIL ID       |    MOBILE NO   | COURSE       |   SEMESTER         |")
            
                print("|"+"-"*130+"|")
                p=9-len(r[0])
                x=9-len(r[1])
                y=11-len(str(r[2]))
                z=6-len(r[3])
                w=12-len(str(r[4]))
                a=13-len(str(r[5]))
                b=11-len(str(r[6]))
                c=4-len(str(r[7]))
                
                print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*" ","|",\
                      r[5],a*" ","|",r[6],b*" ","|",r[7],"\t\t  ","|")
                
                print("|"+"-"*130+"|")
        print(" ")
        print(" ")

        if found==1:
            query="Delete from student_record1 where enrollmentno='{}'".format(eno)      #parameterised query
            mycur.execute(query)
            print("student details after deletion of a record is:")
            display_records()
            con.commit()
        else:
            print("student record not found!!!")
            
    except Exception as e:
        print("Something went wrong!!!",e)

def update_student():      #Updates record of a particular student
    try:
        eno=input("Enter student enrollment no. you want to update:")
        query="Select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        
        for r in row:
            if eno==r[0]:
                found=1
                print(" ")
                print("Student record to be update:")
                print(" ")
                print("|"+"-"*120+"|")
        
                print("| ENROLLMENT NO |     NAME      |      DOB     | GENDER      | EMAIL ID |    MOBILE NO   |\
        COURSE   |   SEMESTER  |")
            
                print("|"+"-"*120+"|")
                p=9-len(r[0])
                x=9-len(r[1])
                y=11-len(str(r[2]))
                z=6-len(r[3])
                w=12-len(str(r[4]))
                a=13-len(str(r[5]))
                b=11-len(str(r[6]))
                c=4-len(str(r[7]))
                
                print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*" ","|",\
                      r[5],a*" ","|",r[6],b*" ","|",r[7],"\t","|")
                
                print("|"+"-"*120+"|")
        print(" ")
        print(" ")
        if found==1:
            course=input("enter course :  ")
            sem=int(input("enter semester :  "))
            mobile=int(input("enter mobile number :  "))
            query="Select * from student_record1"
            mycur.execute(query)
            row=mycur.fetchall()
            query="Update student_record1 set course='{}',semester={},mobileno={} where enrollmentno='{}'".format(course,sem,mobile,eno)          #
            mycur.execute(query)
            con.commit()
            print("Updated Successfully!!!")
        else:
            print("student Record not found!!!")

    except Exception as e:
        print("Something went wrong!!!",e)


def course_record():      #creating course record for all
    try:
        sem=int(input("Enter semester ="))
        brch=input("Enter branch=")
        crs1=input("Enter 1st course=")
        crs2=input("Enter 2nd course=")
        crs3=input("Enter 3rd course=")
        crs4=input("Enter 4th course=")
        crs5=input("Enter 5th course=")
        crs6=input("Enter 6th course=")
        rec=[sem,brch,crs1,crs2,crs3,crs4,crs5,crs6]
        q="Insert into course_record values(%s,%s,%s,%s,%s,%s,%s,%s)"
        mycur.execute(q,rec)
        con.commit()
        print("Data enetr successfully!!!")
    except Exception as e:
        print("Something went wrong!!!",e)

def display_course_records():       #Display all the course records present in the table
    try:
        query="select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        
        print(" ")
        print("|"+"-"*120+"|")
        
        print("| SEMESTER |    BRANCH      |   COURSE1    |   COURSE2    |    COURSE3     |   COURSE4    |\
    COURSE5   |    COURSE6    |")
        
        print("|"+"-"*120+"|")
        
        for r in row:
            p=4-len(str(r[0]))
            x=10-len(str(r[1]))
            y=11-len(str(r[2]))
            z=11-len(str(r[3]))
            w=11-len(str(r[4]))
            a=11-len(str(r[5]))
            b=11-len(str(r[6]))
            c=11-len(str(r[7]))
            
            print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*" ","|",\
                  r[5],a*" ","|",r[6],b*" ","|",r[7],"\t\t","|")
            
        print("|"+"-"*120+"|")
        print(" ")
        print(" ")
        
    except Exception as e:
        print("something went wrong!!!",e)

def update_course_record():    #update course for all 
    try:
        sem=int(input("enter semester="))
        brch=input("enter branch=")
        query="Select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        
        for i in row:
            if sem==i[0] and brch==i[1]:
                found=1
                
        if found==1:
            crs1=input("Enter 1st updated cousre=")
            crs2=input("Enter 2nd updated cousre=")
            crs3=input("Enter 3rd updated cousre=")
            crs4=input("Enter 4th updated cousre=")
            crs5=input("Enter 5th updated cousre=")
            crs6=input("Enter 6th updated cousre=")
            query="Select * from course_record"
            mycur.execute(query)
            row=mycur.fetchall()
            query="Update course_record set course1='{}',course2='{}',course3='{}',course4='{}',course5='{}',course6='{}' where branch='{}' and semester={}".format(crs1,crs2,crs3,crs4,crs5,crs6,brch,sem)          #
            mycur.execute(query)
            print("Student details after updation of a record is:")
            con.commit()
            print("Updated Successfully")
        else:
            print("student Record not found!!!")

    except Exception as e:
        print("Something went wrong!!!",e)


def delete_course():  #delete a particular record
    try:
        sem=int(input("Enter semester ="))
        brch=input("Enter branch=")
        query="Select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        for i in row:
            if sem==i[0] and brch==i[1]:
                found=1
                sql="Delete from course_record where semester={} and branch='{}'".format(sem,brch)
                mycur.execute(sql)
                con.commit()
                print("Deleted successfully!!!")
        if found==0:
            print("Student Record not found")
    except Exception as e:
        print("Something went wrong!!!",e)

def assign_record():    #assign course to a particular student
    try:
        enrl=int(input("enter Your enrollment no.="))
        sem=int(input("enter semester="))
        brch=input("enter branch=")
        query="Select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        for i in row:
            if sem==i[0] and brch==i[1]:
                found=1
                print("1.",i[2],"\n2.",i[3],"\n3.",i[4],"\n4.",i[5],"\n5.",i[6],"\n6.",i[7])
                choice="yes"
                ask=input("Want to assign to student?(yes/no)=")
                if(ask=="yes"):
                    print("*********************ASSIGNED********************")
        if found==0:
            print("Student Record not found")
    except Exception as e:
        print("Something went wrong!!!",e)


def create_exam_record():
    try:
        enrl=input("Enter enrollment no.=")
        sem=int(input("Enter semester="))
        brch=input("Enter branch=")
        query="Select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        for i in row:
            if i[0]==enrl and i[6]==brch and i[7]==sem:
                found=1
                sql="Select * from course_record"
                mycur.execute(sql)
                rec=mycur.fetchall()
                found1=0
                for j in rec:
                    if j[0]==sem and j[1]==brch:
                        found1=1
                        print("Enter marks of ",j[2],end=" ")
                        mrk1=int(input("="))
                        print("Enter marks of ",j[3],end=" ")
                        mrk2=int(input("="))
                        print("Enter marks of ",j[4],end=" ")
                        mrk3=int(input("="))
                        print("Enter marks of ",j[5],end=" ")
                        mrk4=int(input("="))
                        print("Enter marks of ",j[6],end=" ")
                        mrk5=int(input("="))
                        print("Enter marks of ",j[7],end=" ")
                        mrk6=int(input("="))
                        total_mrk=mrk1+mrk2+mrk3+mrk4+mrk5+mrk6
                        perc=total_mrk/6
                        if(perc>90):
                            grd='A'
                        elif(perc>80 and perc<90):
                            grd='B'
                        elif(perc>70 and perc<80):
                            grd='C'
                        elif(perc>60 and perc<70):
                            grd='D'
                        elif(perc>50 and perc<60):
                            grd='E'
                        else:
                            grd='Fail'
                        q="Insert into exam_record values('{}',{},'{}',{},{},{},{},{},{},{},{},'{}')".format(enrl,sem,brch,mrk1,mrk2,mrk3,mrk4,mrk5,mrk6,total_mrk,perc,grd)
                        mycur.execute(q)
                        con.commit()
                if found1==0:
                    print("No such semester or branch found")
                else:
                    print("Marks entered successfully!!!")
        if found==0:
            print("student Record not found!!!")
    except Exception as e:
        print("something went wrong!!!",e)


def display_record():       #Display result of a student
    #try:
        sem=int(input("enter semester="))
        brch=input("enter branch=")
        eno=input("enter enrollment number ")
        query="Select * from student_record1"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        for r in row:
            if eno==r[0]:
                found=1
                print(" ")
                print("|"+"-"*80+"|")
                print("\t\t\tSTUDENT EXAM REPORT")
                print("|"+"-"*80+"|")
                print("\n")
                print(" NAME           :   ",r[1],end="\t\t\t")
                print(" ENROLLMENT NUMBER  :   ",r[0])
                print(" GENDER         :   ",r[3],end="\t\t\t\t")
                print(" COURSE             :   ",r[6])
                      
                print(" DATE_OF_BIRTH  :   ",r[2],end="\t\t\t")
                print(" SEMESTER           :   ",r[7],end="\t\t")
                print("\n\n")
        query1="Select * from course_record"
        mycur.execute(query1)
        row1=mycur.fetchall()
        
        for r in row1:
            if sem==r[0] and brch==r[1]:
                      print(" ")
                      print("|"+"-"*80+"|")
                      print("|  SUBJECT_NAME   |      MARKS     |   OUT_OF   |   PERCENTAGE   |    GRADE      | ")
                      print("|"+"-"*80+"|")
                      query2="Select * from exam_record"
                      mycur.execute(query2)
                      row2=mycur.fetchall()
                        
                      for i in row2:
                          if eno==i[0] and sem==i[1] and brch==i[2]:
                              print("|   ",r[2],(11-len(str(r[2])))*" ","|  ",i[3],(11-len(str(i[3])))*" ","|     100    |     ",(8-len(str(i[10])))*" ","   |   ","\t","        |")
                              print("|   ",r[3],(11-len(str(r[3])))*" ","|  ",i[4],(11-len(str(i[4])))*" ","|     100    |     ",(8-len(str(i[10])))*" ","   |   ","\t","        |")
                              print("|   ",r[4],(11-len(str(r[4])))*" ","|  ",i[5],(11-len(str(i[5])))*" ","|     100    |   ",i[10],(8-len(str(i[10])))*" ","  |   ",i[11],"  ","      |")
                              print("|   ",r[5],(11-len(str(r[5])))*" ","|  ",i[6],(11-len(str(i[6])))*" ","|     100    |     ",(8-len(str(i[10])))*" ","   |   ","\t","        |")
                              print("|   ",r[6],(11-len(str(r[6])))*" ","|  ",i[7],(11-len(str(i[7])))*" ","|     100    |     ",(8-len(str(i[10])))*" ","   |   ","\t","        |")
                              print("|   ",r[7],(11-len(str(r[7])))*" ","|  ",i[8],(11-len(str(i[8])))*" ","|     100    |     ",(8-len(str(i[10])))*" ","   |   ","\t","        |")
                              print("|"+"-"*80+"|")
                              print("\t\t TOTAL_MARKS =  ",i[9])






def display_exam_records():       #Display records of student coursewise
    try:
        sem=int(input("enter semester="))
        brch=input("enter branch=")
        query="Select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        
        for r in row:
            if sem==r[0] and brch==r[1]:
                found=1
        
                print(" ")
                print("|"+"-"*165+"|")
                
                print("|ENROLLMENT NO | SEMESTER |    BRANCH      | ",r[2]," |",r[3],"   |",r[4],"   |  ",r[5],"     |",r[6],"    |",r[7],"|  TOTAL_MARKS   |   PERCENTAGE   |    GRADE   |")
                
                print("|"+"-"*165+"|")

        query1="Select * from exam_record"
        mycur.execute(query1)
        row1=mycur.fetchall()
        
        for r in row1:
            if sem==r[1] and brch==r[2]:
                p=8-len(str(r[0]))
                x=4-len(str(r[1]))
                y=13-len(str(r[2]))
                z=8-len(str(r[3]))
                w=9-len(str(r[4]))
                a=11-len(str(r[5]))
                b=8-len(str(r[6]))
                c=7-len(str(r[7]))
                d=6-len(str(r[8]))
                e=14-len(str(r[9]))
                f=13-len(str(r[10]))
                g=9-len(str(r[11]))
                
                print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*" ","|",\
                      r[5],a*" ","|",r[6],b*" ","|",r[7],c*" ","|",r[8],d*" ","|",r[9],e*" ","|",r[10],f*" ","|",r[11],"\t     ","|")
                
        print("|"+"-"*165+"|")
        print(" ")
        print(" ")
        query="Select AVG(total_marks) from exam_record where semester={} and branch='{}'".format(sem,brch)
        mycur.execute(query)
        a=mycur.fetchone()
        print("AVERAGE marks of all students(Coursewise)=",a[0])
        query="Select Count(*) from exam_record "
        mycur.execute(query)
        a=mycur.fetchone()
        print("Total no. of students=",a[0])
        
    except Exception as e:
        print("something went wrong!!!",e)
    

def display_overall_result():       #Display all the records present in the table
    try:
        query="Select * from course_record"
        mycur.execute(query)
        row=mycur.fetchall()
        print(" ")
        print("|"+"-"*184+"|")
        print("|  ENROL_NO |  SEMESTER  |   BRANCH |  SUBJECT1  |   SUBJECT2   |   SUBJECT3   |   SUBJECT4    |    SUBJECT5   |    SUBJECT6   |    MARKS OBTAINED   |    PERCENTAGE    |     GRADE      | ")
        print("|"+"-"*184+"|")
        for i in row:

                query1="Select * from exam_record"
                mycur.execute(query1)
                row1=mycur.fetchall()
                
                for r in row1:
                    if i[0]==r[1] and i[1]==r[2]:
                        p=5-len(str(r[0]))
                        x=6-len(str(r[1]))
                        y=7-len(str(r[2]))
                        z=9-len(str(r[3]))
                        w=9-len(str(r[4]))
                        a=11-len(str(r[5]))
                        b=12-len(str(r[6]))
                        c=12-len(str(r[7]))
                        d=12-len(str(r[8]))
                        e=18-len(str(r[9]))
                        f=15-len(str(r[10]))
                        g=11-len(str(r[11]))
                    
                        print("|   ",r[0],p*" ","|","  ",r[1],x*" ","|",r[2],y*" ","|",r[3],z*" ","|  ",r[4],w*" ","|",\
                              r[5],a*" ","|",r[6],b*" ","|",r[7],c*" ","|",r[8],d*" ","|",r[9],e*" ","|",r[10],f*" ","|",r[11],"\t\t","|")
                        
        print("|"+"-"*184+"|")
        print(" ")
        print(" ")

        query="Select AVG(total_marks) from exam_record "
        mycur.execute(query)
        a=mycur.fetchone()
        print("AVERAGE marks of all students=",a[0])
        query="Select Count(*) from exam_record "
        mycur.execute(query)
        a=mycur.fetchone()
        print("Total no. of students=",a[0])
        
    except Exception as e:
        print("something went wrong!!!",e)
    

def update_exam_record():
    try:
        enrl=input("Enter enrollment no.=")
        sem=int(input("enter semester="))
        brch=input("enter branch=")
        query="Select * from exam_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        
        for i in row:
            if enrl==i[0] and sem==i[1] and brch==i[2]:
                found=1
                q="Select * from course_record"
                mycur.execute(q)
                f=mycur.fetchall()
                for j in f:
                    if j[0]==sem and j[1]==brch:
                        print("Enter  new marks of ",j[2],end=" ")
                        mr1=int(input("="))
                        print("Enter new marks of ",j[3],end=" ")
                        mr2=int(input("="))
                        print("Enter new marks of ",j[4],end=" ")
                        mr3=int(input("="))
                        print("Enter new marks of ",j[5],end=" ")
                        mr4=int(input("="))
                        print("Enter new marks of ",j[6],end=" ")
                        mr5=int(input("="))
                        print("Enter new marks of ",j[7],end=" ")
                        mr6=int(input("="))
                        total_mr=mr1+mr2+mr3+mr4+mr5+mr6
                        per=total_mr/6
                        if(per>90):
                            gd='A'
                        elif(per>80 and per<90):
                            gd='B'
                        elif(per>70 and per<80):
                            gd='C'
                        elif(per>60 and per<70):
                            gd='D'
                        elif(per>50 and per<60):
                            gd='E'
                        else:
                            gd='Fail'
                        q="update exam_record set mark_sub1={},mark_sub2={},mark_sub3={},mark_sub4={},mark_sub5={},mark_sub6={},total_marks={},percentage={},grade='{}' where enrollmentno='{}' and semester={} and branch='{}'".format(mr1,mr2,mr3,mr4,mr5,mr6,total_mr,per,gd,enrl,sem,brch)
                        mycur.execute(q)
                        con.commit()

        if found==1:                    
            print("Updated Successfully")
        else:
            print("student Record not found!!!")

    except Exception as e:
        print("Something went wrong!!!",e)


def delete_exam_record():
    try:
        enrl=input("Enter enrollment no.=")
        sem=int(input("Enter semester ="))
        brch=input("Enter branch=")
        query="Select * from exam_record"
        mycur.execute(query)
        row=mycur.fetchall()
        found=0
        for i in row:
            if enrl==i[0] and brch==i[2] and sem==i[1]:
                found=1
                sql="Delete from exam_record where enrollmentno='{}' and semester={} and branch='{}'".format(enrl,sem,brch)
                mycur.execute(sql)
                con.commit()
                print("Deleted successfully!!!")
        if found==0:
            print("Student Record not found")
    except Exception as e:
        print("Something went wrong!!!",e)


while True:
    print("\n\n\n")
    print("*"*80)
    print("\t\t\tDASHBOARD")
    print("*"*80)
    print("\t1.  STUDENT RECORD")
    print("\t2.  COURSE RECORD")
    print("\t3.  EXAM RECORD")
    print("\t4.  EXIT")
    ch=int(input("which record would you like to see : "))
    if ch==1:
        print("\t1.  Create a student record")
        print("\t2.  Display a student record")
        print("\t3.  Display record of all students")
        print("\t4.  Delete a student record")
        print("\t5.  Update a student record")
        ch2=int(input("Enter your choice : "))
        if ch2==1:
            createrec()
        elif ch2==2:
            display_student_record()
        elif ch2==3:
            display_records()
        elif ch2==4:
            delete_student()
        elif ch2==5:
            update_student()
        else:
            print("Invalid choice")
    elif ch==2:
        print("\t1.  Create a course record")
        print("\t2.  Display a course record")
        print("\t3.  Update a course record")
        print("\t4.  Delete a course record")
        print("\t5.  Assign record to a student")
        ch2=int(input("Enter your choice : "))
        if ch2==1:
            course_record()
        elif ch2==2:
            display_course_records()
        elif ch2==3:
            update_course_record()
        elif ch2==4:
            delete_course()
        elif ch2==5:
            assign_record()
        else:
            print("Invalid choice")
    elif ch==3:
        print("\t1.  Create exam record of a student")
        print("\t2.  Display result of a particular student")
        print("\t3.  Display exam record of a particular branch and semester")
        print("\t4.  Display exam record of all student")
        print("\t5.  Update exam record of a student")
        print("\t6.  Delete exam record of a student")
        ch2=int(input("\tEnter your choice : "))
        if ch2==1:
            create_exam_record()
        elif ch2==2:
            display_record()
        elif ch2==3:
            display_exam_records()
        elif ch2==4:
            display_overall_result()
        elif ch2==5:
            update_exam_record()
        elif ch2==6:
            delete_exam_record()
        else:
            print("Invalid choice")
    elif ch==4:
        print("**************THANK YOU**************")
        break;
    else:
        print("invalid choice")
        break
