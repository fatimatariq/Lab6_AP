# Lab6_AP
//Author : FATIMA TAROQ(697)
// LAB 6


package lab6;

import java.util.*;
import java.io.*;


 
public class fileCrawler {
 
   WorkQueue Queue;
 
 
  class Worker_thread implements Runnable {
 
   WorkQueue queue;
 
  public Worker_thread(WorkQueue instanceWorkQueue) 
  {
   queue = instanceWorkQueue;
  }
 
 
  public void run() {
   String name_of_file;
   
   while ((name_of_file = queue.remove_entry()) != null)
   {
    File file_name = new File(name_of_file);
    String stuff[] = file_name.list();
    if (stuff == null)
    {
     continue;
    }
    for (String entry : stuff)
    {
     if (entry.compareTo(".") == 0)
     {
      continue;
     }
     if (entry.compareTo("..") == 0)
     {
      continue;
     }
     String output = name_of_file + "\\" + entry;
     System.out.println(output);
    }
   }
  }
 }
 
 public fileCrawler() 
 {
	 Queue = new WorkQueue();
 }
 
 public Worker_thread Worker()
 {
  return new Worker_thread(Queue);
 }
 
 
 public void Dir(String process_directory)
 {
   try{
   File file = new File(process_directory);
   if (file.isDirectory()) {
    String entries[] = file.list();
    if (entries != null)
    	Queue.add_entry(process_directory);
 
    for (String entry : entries) 
    {
     String subdir;
     if (entry.compareTo(".") == 0)
     {
      continue;
     }
     if (entry.compareTo("..") == 0)
     {
      continue;
     }
     if (process_directory.endsWith("\\"))
      subdir = process_directory+entry;
     else
      subdir = process_directory+"\\"+entry;
     Dir(subdir);
    }
   }
   }
   catch(Exception e){}
 }
 
 public static void main(String Args[])
 {
 
	 
	 int N = 5;
  fileCrawler fc = new fileCrawler();
 
     
  ArrayList<Thread> thread = new ArrayList<Thread>(N);  
  for (int m = 0; m < N; m++)
  {
   Thread thread1 = new Thread(fc.Worker());
   thread.add(thread1);
   thread1.start();   //   start  the worker threads
  }
 

  fc.Dir("C:\\Users/Fatima/Downloads");  //  place each directory 
 
 
  fc.Queue.exit_quit();  //  no more directories to add
 
  for (int k = 0; k < N; k++)
  {
   try 
   {
    thread.get(k).join();
   }
   catch (Exception e) {};
  }
 }
}
