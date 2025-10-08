input:

#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <sstream>

using namespace std;

struct Task {
    string description;
    bool completed;
};

// Function declarations
void loadTasks(vector<Task>& tasks);
void saveTasks(const vector<Task>& tasks);
void addTask(vector<Task>& tasks);
void viewTasks(const vector<Task>& tasks);
void completeTask(vector<Task>& tasks);

const string FILENAME = "tasks.txt";

int main() {
    vector<Task> tasks;
    loadTasks(tasks);

    int choice;
    do {
        cout << "\n=== Task Tracker Menu ===\n";
        cout << "1. Add Task\n";
        cout << "2. View Tasks\n";
        cout << "3. Complete Task\n";
        cout << "4. Exit\n";
        cout << "Choose an option (1-4): ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                addTask(tasks);
                break;
            case 2:
                viewTasks(tasks);
                break;
            case 3:
                completeTask(tasks);
                break;
            case 4:
                saveTasks(tasks);
                cout << "Tasks saved. Goodbye!\n";
                break;
            default:
                cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 4);

    return 0;
}

// Load tasks from file
void loadTasks(vector<Task>& tasks) {
    ifstream file(FILENAME);
    if (!file) {
        // File doesn't exist yet
        return;
    }

    string line;
    while (getline(file, line)) {
        stringstream ss(line);
        string desc, statusStr;
        getline(ss, desc, '|');
        getline(ss, statusStr);

        Task t;
        t.description = desc;
        t.completed = (statusStr == "1");
        tasks.push_back(t);
    }

    file.close();
}

// Save tasks to file
void saveTasks(const vector<Task>& tasks) {
    ofstream file(FILENAME);
    for (const auto& task : tasks) {
        file << task.description << "|" << (task.completed ? "1" : "0") << endl;
    }
    file.close();
}

// Add a new task
void addTask(vector<Task>& tasks) {
    Task t;
    cout << "Enter task description: ";
    getline(cin, t.description);
    t.completed = false;
    tasks.push_back(t);
    cout << "Task added!\n";
}

// View all tasks
void viewTasks(const vector<Task>& tasks) {
    if (tasks.empty()) {
        cout << "No tasks found.\n";
        return;
    }

    cout << "\n=== Your Tasks ===\n";
    for (size_t i = 0; i < tasks.size(); ++i) {
        cout << i + 1 << ". [" << (tasks[i].completed ? "X" : " ") << "] " << tasks[i].description << endl;
    }
}

// Mark a task as completed
void completeTask(vector<Task>& tasks) {
    if (tasks.empty()) {
        cout << "No tasks to complete.\n";
        return;
    }

    viewTasks(tasks);
    int num;
    cout << "Enter the number of the task to mark as completed: ";
    cin >> num;
    cin.ignore();

    if (num < 1 || num > tasks.size()) {
        cout << "Invalid task number.\n";
        return;
    }

    if (tasks[num - 1].completed) {
        cout << "Task is already completed.\n";
    } else {
        tasks[num - 1].completed = true;
        cout << "Task marked as completed!\n";
    }
}

output:

=== Task Tracker Menu ===
1. Add Task
2. View Tasks
3. Complete Task
4. Exit
Choose an option (1-4): 
