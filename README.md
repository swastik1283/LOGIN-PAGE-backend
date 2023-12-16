# LOGIN-PAGE-backend

import tkinter
from tkinter import*
import tkinter.messagebox as messagebox
from PIL import ImageTk,Image
import mysql.connector as sql
import mysql


mydb = mysql.connector.connect(host="localhost",user="root",password="Saksham-034",database="swas")

#insert button functioning

def insert():
    _stu_id=student_id_entry.get()
    _name=name_entry.get()
    _contact=contact_entry.get()  

    if(_stu_id=="" or _name=="" or _contact==""):
        messagebox.showinfo("insert err","all field are required")
    elif(student_id==get_by_id()):
        messagebox.showinfo("already exist ") 
    else:
        mycursor=mydb.cursor()
        sql="insert into student values(%s,%s,%s)"
        data=(_stu_id,_name,_contact)
        mycursor.execute(sql,data)
        mydb.commit()
        student_id_entry.delete(0,'end')
        name_entry.delete(0,'end')
        contact_entry.delete(0,'end')
        row=mycursor.rowcount
        show()
        messagebox.showinfo("successful",f"{row}row is inserted")
        
#delete button functioning

def delete():
    if(student_id_entry.get()==""):
        messagebox.showinfo("error","Insert the valid value")
    else:
        mycursor=mydb.cursor()
        mycursor.execute("delete from student where student_id ="+student_id_entry.get()+"")
        mydb.commit()
        student_id_entry.delete(0,'end')
        name_entry.delete(0,'end')
        contact_entry.delete(0,'end')
        row=mycursor.rowcount
        show()
        messagebox.showinfo("successful",f"{row}Row deleted ")                               

#update button functioning

def update():
    student_id = student_id_entry.get()
    name = name_entry.get()
    contact = contact_entry.get()

    if student_id == "" or name == "" or contact == "":
        messagebox.showinfo("invalid entry")
    else:
        mycursor = mydb.cursor()
        mycursor.execute("update student set name = %s, contact = %s where student_id = %s", (name_entry.get(), contact_entry.get(), student_id))
        mydb.commit()
        student_id_entry.delete(0, 'end')
        name_entry.delete(0, 'end')
        contact_entry.delete(0, 'end')
        row = mycursor.rowcount
        show()
        messagebox.showinfo("successful", f"{row} row updated")

#getbyid 

def get_by_id():
    if(student_id_entry.get()==""):
         messagebox.showinfo("get status ",'enter student_id for fetch result ')
    else:
       mycursor = mydb.cursor()
       mycursor.execute("select * from student where student_id= '" + student_id_entry.get() +"'")
       result=mycursor.fetchall() 
       if result:
        return True 
       else :
        return False
       for row in result:
            name_entry.insert(0,row[1])
            contact_entry.insert(0,row[2])
       mydb.commit()
       
#get by button functioning

def get():
    if(student_id_entry.get()==""):
         messagebox.showinfo("get status ",'enter student_id for fetch result ')
    else:
       mycursor = mydb.cursor()
       mycursor.execute("select * from student where Student_ID= '" + student_id_entry.get() +"'")
       result=mycursor.fetchall() 
       for row in result:
            name_entry.insert(0,row[1])
            contact_entry.insert(0,row[2])
       mydb.commit()

#show button functioning

def show():
         mycursor = mydb.cursor()
         mycursor.execute("select * from student")
         result=mycursor.fetchall() 
         mylist.delete(0,mylist.size())
         for row in result:
             insertdata=str(row[0])+'   '+str(row[1])+'    '+str(row[2])
             mylist.insert(mylist.size()+1,insertdata)           


root = Tk()

root.title("Login Page")
root.geometry("800x500")
root.maxsize(800,500)
root.minsize(800,500)

def showdetails():
    print(f"ID is: {student_id_entry.get()} and Name is: {name_entry .get()}")


student_id = Label(text="Student_ID")
student_id.place(x=50, y=20)

student_id_entry=Entry() 

student_id_entry.place(x=120, y=20)

name = Label(text="Name")

name.place(x=50, y=50)

name_entry = Entry()
name_entry.place(x=120, y=50)

contact=Label(text="contact")
contact.place(x=50,y=90)
contact_entry=Entry()
contact_entry.place(x=120,y=90)

#button creation 

submitbtn = Button(text="Submit", command=showdetails)
submitbtn.place(x=50, y=120)
insert = Button(root,text="Insert", font=('lucida',10,'bold'),bg="white", command=insert)
insert.place(x=20,y=160)

delete = Button(root,text="Delete", font=('lucida',10,'bold'),bg="white",command=delete)
delete.place(x=70,y=160)

update = Button(root,text="Update", font=('lucida',10,'bold'),bg="white",command=update)
update.place(x=130,y=160)

get = Button(root,text="Get", font=('lucida',10,'bold'),bg="white",command=get)
get.place(x=190,y=160)

mylist = Listbox(root)
mylist.place(x=300, y=30, width=230)
showdetails()



root.mainloop()
