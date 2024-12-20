# FileHandlingAndSortingStudentRecords
It includes file handling and sorting

package com.company;

public class Main {

    public static void main(String[] args) {
        StudentRecord sr = new StudentRecord();

        sr.add("2312203","Muhammad","Maaz","maaz@gmail.com","02134812878");
        sr.add("2312203","Muhammad","Khuzaimah","Khuzaimah@gmail.com","0213481478");
        sr.add("2312203","Muhammad","Ifham","ifham@gmail.com","021348128847");
        sr.add("2312203","Mustafa","Murtaza","mustafa@gmail.com","02134782878");
        sr.add("2312203","Malik","Mushtaq","malik@gmail.com","02134812988");
        sr.add("2312203","Abdul","Hadi","hadi@gmail.com","02134819988");

        int i=0;
        while (i<StudentRecord.pointer){
            FileHandling.fwriter(i++);
        }
        sr.print();
    }
}


package com.company;

public class StudentRecord {
    public static String [][] students;
    public static int pointer;
    StudentRecord(){
        students = new String[50][5];
        pointer= 0;
    }

    public void add(String StudendID, String fName, String lName, String email, String phonenumber){
        students[pointer][0]=StudendID;
        students[pointer][1]= fName;
        students[pointer][2]= lName;
        students[pointer][3]= email;
        students[pointer++][4]= phonenumber;
        sort();

    }
    public void sort(){
        //int length= arr.length;
        for (int i=0; i<pointer;i++) {

            for (int j = 0; j < pointer-i-1; j++) {

                if (students[j][1].charAt(0) > students[j + 1][1].charAt(0)) {
                    String[] temp = students[j];
                    students[j] = students[j + 1];
                    students[j + 1] = temp;

                } else if (students[j][1].charAt(0) == students[j + 1][1].charAt(0)) {

                    if (students[j][2].charAt(0) > students[j + 1][2].charAt(0)) {

                        String[] temp = students[j];
                        students[j] = students[j + 1];
                        students[j + 1] = temp;
                    }
                }
            }
        }


    }
    public String search(String fname,String lname){
        for (int i=0;i< pointer;i++){
            if (students[i][1].equals(fname) && students[i][2].equals(lname)){
                return"Student ID: "+ students[i][0] +"\nFirst Name: "+students[i][1]+"\nLast Name: "+students[i][2]+"\nEmail: "+students[i][3]+"\nPhone Number:"+students[i][4];
            }
        }
        return"Student Not Found";
    }
    public void print(){

       // for (int i = 0;i<pointer;i++){
//            System.out.println("Student ID: "+ students[i][0] +"\nFirst Name: "+students[i][1]+"\nLast Name: "+students[i][2]+"\nEmail: "+students[i][3]+"\nPhone Number:"+students[i][4]);
//            System.out.println();
            FileHandling.freader(pointer);
      //  }

    }
}


package com.company;
import java.io.*;
import java.util.Scanner;

public class FileHandling {


        public static void fwriter(int ind) {
            String data = StudentRecord.students[ind][0] + "," + StudentRecord.students[ind][1] + "," + StudentRecord.students[ind][2] + "," + StudentRecord.students[ind][3] + "," + StudentRecord.students[ind][4] + "\n";

            try {
                BufferedWriter write = new BufferedWriter(new FileWriter("Student.csv", true));

                write.write(data);
                write.close();
            } catch (IOException e) {
                System.out.println("Error While Writing");
            }
        }

        public static void freader(int ind) {

            String data = "";

            try {
                BufferedReader reader = new BufferedReader(new FileReader("Student.csv"));
                while ((data = reader.readLine()) != null) {

                    Scanner sc = new Scanner(data);
                    sc.useDelimiter(",");

                    StudentRecord.students[ind][0] = sc.next();
                    StudentRecord.students[ind][1] = sc.next();
                    StudentRecord.students[ind][2] = sc.next();
                    StudentRecord.students[ind][3] = sc.next();
                    StudentRecord.students[ind][4] = sc.next();
                    System.out.println("Student ID: "+ StudentRecord.students[ind][0]
                            +"\nFirst Name: "+StudentRecord.students[ind][1]+
                            "\nLast Name: "+StudentRecord.students[ind][2]+
                            "\nEmail: "+StudentRecord.students[ind][3]+
                            "\nPhone Number:"+StudentRecord.students[ind][4]);
                   // System.out.println(data);
                    System.out.println();
                    StudentRecord.pointer++;
                }
                reader.close();

            } catch (FileNotFoundException e) {
                throw new RuntimeException(e);
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
}
