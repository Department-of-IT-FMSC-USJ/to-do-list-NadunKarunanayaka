[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/WPrEv7v0)
using System;
using System.Collections.Generic;

public class Task
{
    public string TaskID { get; set; }
    public string TaskName { get; set; }
    public string Description { get; set; }
    public DateTime Date { get; set; }
    public string Status { get; set; }

    public Task(string id, string name, string description, DateTime date)
    {
        TaskID = id;
        TaskName = name;
        Description = description;
        Date = date;
        Status = "To Do"; // Initial status
    }

    public override string ToString()
    {
        return $"{TaskID} | {TaskName} | {Description} | {Date.ToShortDateString()} | {Status}";
    }
}

public class TaskManager
{
    public LinkedList<Task> ToDoList = new LinkedList<Task>();
    public Stack<Task> InProgressStack = new Stack<Task>();
    public Queue<Task> CompletedQueue = new Queue<Task>();

    // Insert task into To-Do list in ascending order of date
    public void InsertTask(Task task)
    {
        if (ToDoList.Count == 0)
        {
            ToDoList.AddFirst(task);
            return;
        }

        var current = ToDoList.First;
        while (current != null && current.Value.Date <= task.Date)
        {
            current = current.Next;
        }

        if (current == null)
            ToDoList.AddLast(task);
        else
            ToDoList.AddBefore(current, task);
    }

    // Change task status to "In Progress" and push to stack
    public void StartTask(string taskId)
    {
        var node = ToDoList.First;
        while (node != null)
        {
            if (node.Value.TaskID == taskId)
            {
                Task task = node.Value;
                task.Status = "In Progress";
                InProgressStack.Push(task);
                ToDoList.Remove(node);
                return;
            }
            node = node.Next;
        }
    }

    // Change task status to "Completed" and move from stack to queue
    public void CompleteTask()
    {
        if (InProgressStack.Count > 0)
        {
            Task task = InProgressStack.Pop();
            task.Status = "Completed";
            CompletedQueue.Enqueue(task);
        }
    }

    // Print all task lists
    public void PrintAllLists()
    {
        Console.WriteLine("\n--- To Do Tasks ---");
        foreach (var t in ToDoList)
            Console.WriteLine(t);

        Console.WriteLine("\n--- In Progress Tasks ---");
        foreach (var t in InProgressStack)
            Console.WriteLine(t);

        Console.WriteLine("\n--- Completed Tasks ---");
        foreach (var t in CompletedQueue)
            Console.WriteLine(t);
    }
}

class Program
{
    static void Main(string[] args)
    {
        TaskManager manager = new TaskManager();

        // Sample tasks for 29th May 2025
        Task task1 = new Task("T001", "Develop a Student Database System",
            "Design and implement a simple student database system using SQL and C# to manage student records including CRUD operations.",
            new DateTime(2025, 5, 29));

        Task task2 = new Task("T002", "Design Network Topology for Department Lab",
            "Create a logical network topology using Cisco Packet Tracer for the IT lab with proper IP addressing and routing configurations.",
            new DateTime(2025, 5, 29));

        Task task3 = new Task("T003", "UML Diagrams for Inventory Management System",
            "Draw use case, class, and sequence diagrams for a departmental inventory management system using any UML design tool.",
            new DateTime(2025, 5, 29));

        // Insert tasks into To-Do list
        manager.InsertTask(task1);
        manager.InsertTask(task2);
        manager.InsertTask(task3);

        // Move a task to In Progress and Complete it
        manager.StartTask("T001");
        manager.CompleteTask();

        // Print all task lists
        manager.PrintAllLists();
    }
}
