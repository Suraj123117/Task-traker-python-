# Task-traker-python-
import json
import os
from datetime import datetime

FILE_NAME = "tasks.json"

def load_tasks():
    if not os.path.exists(FILE_NAME):
        return []
    with open(FILE_NAME, "r") as f:
        return json.load(f)

def save_tasks(tasks):
    with open(FILE_NAME, "w") as f:
        json.dump(tasks, f, indent=4)

def add_task(description, due_date):
    tasks = load_tasks()
    tasks.append({
        "id": len(tasks) + 1,
        "description": description,
        "due_date": due_date,
        "done": False
    })
    save_tasks(tasks)

def list_tasks():
    tasks = load_tasks()
    for task in tasks:
        status = "Done" if task["done"] else "Pending"
        print(f"{task['id']}. {task['description']} (Due: {task['due_date']}) - {status}")

def mark_done(task_id):
    tasks = load_tasks()
    for task in tasks:
        if task["id"] == task_id:
            task["done"] = True
            break
    save_tasks(tasks)

def delete_task(task_id):
    tasks = load_tasks()
    tasks = [task for task in tasks if task["id"] != task_id]
    save_tasks(tasks)

def main():
    while True:
        print("\n1. Add Task\n2. List Tasks\n3. Mark as Done\n4. Delete Task\n5. Exit")
        choice = input("Choose an option: ")
        
        if choice == "1":
            desc = input("Enter task description: ")
            due = input("Enter due date (YYYY-MM-DD): ")
            add_task(desc, due)
        elif choice == "2":
            list_tasks()
        elif choice == "3":
            task_id = int(input("Enter task ID to mark as done: "))
            mark_done(task_id)
        elif choice == "4":
            task_id = int(input("Enter task ID to delete: "))
            delete_task(task_id)
        elif choice == "5":
            break
        else:
            print("Invalid choice!")

if __name__ == "__main__":
    main()
