Proje detayları için [Task-Tracker](https://roadmap.sh/projects/task-tracker)

----------

Task Tracker adında yeni bir proje oluşturup **cli**, **model**, **repository**, **service** ve **util** dizinlerini oluşturalım.

```
src
 -cli
 -model
 -repository
 -service
 -util

```

**model** package’ı altına **Task ve TaskStatus** modellerini oluşturarak başlayalım.

**TaskStatus Task**’ın durumunu belirteceği için enum tipinde olabilir.

_TaskStatus.java_

```java
package model;

public enum TaskStatus {
    TODO,
    IN_PROGRESS,
    DONE
}

```

**Task** modeli içinde gerekli değişkenleri tanımlayalıp, constructor, getter ve setter’ları generate edelim. Constructor içerisinde LocalDateTime.now(); ile createdAt ve updatedAt değerlerinin oluşturulurken o anki zaman değerini almasını sağlayabiliriz. Bu nedenle constructor’a parametre olarak gelmesine gerek yok.

_Task.java_

```java
package model;

import java.time.LocalDateTime;

public class Task {
    private int id;
    private String description;
    private TaskStatus status;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
    
    public Task() {
        super();
    }
    
    public Task(int id, String description, TaskStatus status) {
        this.id = id;
        this.description = description;
        this.status = status;
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public TaskStatus getStatus() {
        return status;
    }

    public void setStatus(TaskStatus status) {
        this.status = status;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public void setCreatedAt(LocalDateTime createdAt) {
        this.createdAt = createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }

    public void setUpdatedAt(LocalDateTime updatedAt) {
        this.updatedAt = updatedAt;
    }
}

```

Projeyi geliştirirken herhangi bir framework ya da kütüphane kullanılmaması gerektiği belitrilmiş.

Bu nedenle JSON operasyonlarını kodlamamız gerekecek. **util** altında **JsonUtil** class’ı oluşturalım.

Bu class’ta **Task** modelini JSON’a dönüştüren ve JSON dosyasını da **Task** modeline dönüştüren iki metod yazmamız gerek. parseJsonToTasks metodu okunan JSON dosyasını List<Task> olarak dönecek. convertTaskToJson ise List<Task> alıp JSON’ı String olarak dönecek.

_JsonUtil.java_

```java
package util;

import model.Task;
import model.TaskStatus;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.util.ArrayList;
import java.util.List;

public class JsonUtil {
    public static List<Task> parseJsonToTasks(String jsonString) {
        List<Task> tasks = new ArrayList<>();
        jsonString = jsonString.trim();

        if (jsonString.startsWith("[") && jsonString.endsWith("]")) {
            jsonString = jsonString.substring(1, jsonString.length() - 1).trim();

            String[] taskStrings = jsonString.split("(?<=\\\\})\\\\s*,\\\\s*(?=\\\\{)");

            for (String taskString : taskStrings) {
                taskString = taskString.trim();
                if (taskString.isEmpty()) continue;

                Task task = new Task();
                taskString = taskString.substring(1, taskString.length() - 1).trim();
                String[] keyValuePairs = taskString.split(",(?=(?:[^\\"]*\\"[^\\"]*\\")*[^\\"]*$)");

                for (String pair : keyValuePairs) {
                    pair = pair.trim();
                    String[] keyValue = pair.split(":", 2);
                    if (keyValue.length != 2) continue;

                    String key = keyValue[0].trim().replaceAll("\\"", "");
                    String value = keyValue[1].trim().replaceAll("\\"", "");

                    try {
                        switch (key) {
                            case "id":
                                task.setId(Integer.parseInt(value));
                                break;
                            case "description":
                                task.setDescription(value);
                                break;
                            case "status":
                                task.setStatus(TaskStatus.valueOf(value));
                                break;
                            case "createdAt":
                                if (!value.equals("null")) {
                                    task.setCreatedAt(LocalDateTime.parse(value, DateTimeFormatter.ISO_LOCAL_DATE_TIME));
                                }
                                break;
                            case "updatedAt":
                                if (!value.equals("null")) {
                                    task.setUpdatedAt(LocalDateTime.parse(value, DateTimeFormatter.ISO_LOCAL_DATE_TIME));
                                }
                                break;
                            default:
                                throw new IllegalArgumentException("Unexpected value: " + key);
                        }
                    } catch (DateTimeParseException e) {
                        System.err.println("Error parsing date-time for key '" + key + "': " + value);
                        continue;
                    }
                }
                tasks.add(task);
            }
        }
        return tasks;
    }

    public static String convertTaskToJson(List<Task> tasks) {
        StringBuilder jsonBuilder = new StringBuilder("[");
        for (int i = 0; i < tasks.size(); i++) {
            Task task = tasks.get(i);
            jsonBuilder.append("{")
                    .append("\\"id\\":").append(task.getId()).append(",")
                    .append("\\"description\\":\\"").append(task.getDescription()).append("\\",")
                    .append("\\"status\\":\\"").append(task.getStatus().toString()).append("\\",")
                    .append("\\"createdAt\\":\\"")
                    .append(task.getCreatedAt() != null ? task.getCreatedAt().format(DateTimeFormatter.ISO_LOCAL_DATE_TIME) : "null")
                    .append("\\",")
                    .append("\\"updatedAt\\":\\"")
                    .append(task.getUpdatedAt() != null ? task.getUpdatedAt().format(DateTimeFormatter.ISO_LOCAL_DATE_TIME) : "null")
                    .append("\\"")
                    .append("}");
            if (i < tasks.size() - 1) {
                jsonBuilder.append(",");
            }
        }
        jsonBuilder.append("]");
        String jsonString = jsonBuilder.toString();
        return jsonString;
    }
}


```

Şimdi **JsonUtil**’i kullanacak **TaskRepository**’i yazalım. Temelde iki işlev var. **repository** altına **TaskRepository** adında interface oluşturalım.

_TaskRepository.java_

```java
package repository;

import model.Task;

import java.util.List;

public interface TaskRepository {
    List<Task> loadTasks();
    void saveTasks(List<Task> tasks);
}


```

Daha sonra **TaskRepostiroy**’i implement eden **TaskRepositoryImpl** class’ını oluşturalım ve loadTasks ve saveTasks metodlarını da implement edelim.

loadTasks JSON dosyasını okumalı (eğer yoksa oluşturmalı) ve List<Task> tipinde dönmeli. Ancak öncesinde JSON path’ini static olarak tanımlamalıyız.

saveTasks ise Task’ları JSON dosyasına yazacak.

_TaskRepositoryImpl.java_

```java
package repository;

import model.Task;
import util.JsonUtil;

import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class TaskRepositoryImpl implements  TaskRepository {

    private static final String FILE_PATH = "tasks.json";

    @Override
    public List<Task> loadTasks() {
        List<Task> tasks = new ArrayList<>();
        File file = new File(FILE_PATH);

        if (!file.exists()) {
            System.out.println("File does not exist. Creating...");
            return tasks;
        }

        try (FileReader reader = new FileReader(file)) {
            StringBuilder jsonString = new StringBuilder();
            int character;
            while ((character = reader.read()) != -1) {
                jsonString.append((char) character);
            }
            tasks = JsonUtil.parseJsonToTasks(jsonString.toString());
        } catch (IOException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
        return tasks;
    }

    @Override
    public void saveTasks(List<Task> tasks) {
        String jsonString = JsonUtil.convertTaskToJson(tasks);
        File file = new File(FILE_PATH);

        try (FileWriter writer = new FileWriter(file)) {
            writer.write(jsonString);
        } catch (IOException e) {
            System.err.println("Error saving tasks: " + e.getMessage());
        }
    }
}

```

Şimdi de projede istenen fonksiyonları gerçekleştirecek servisleri yazalım. **service** altına **TaskService** adında bir interface oluştup kullanılacak metodları yazalım.

_TaskService.java_

```java
package service;

import model.Task;
import model.TaskStatus;

import java.util.List;

public interface TaskService {
    void addTask(String description);
    void updateTask(int id , String description);
    void deleteTask(int id);
    void markInProgressTask(int id);
    void markDoneTask(int id);

    List<Task> listAllTasks();

    List<Task> listTaskByStatus(TaskStatus status);

    void printAllTasks(List<Task> tasks);
}


```

Daha sonra service altına **TaskServiceImpl** class’ını oluşturup **TaskService**’i ve metodları implement edelim. **TaskRepository**’i da tanımlayalıp metodları yazalım.

_TaskServiceImpl.java_

```java
package service;

import model.Task;
import model.TaskStatus;
import repository.TaskRepository;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class TaskServiceImpl implements TaskService {

    private List<Task> tasks = new ArrayList<>();
    private int nextId = 1;
    private TaskRepository taskRepository;

    public TaskServiceImpl(TaskRepository taskRepository) {
        super();
        this.taskRepository = taskRepository;
        this.tasks = taskRepository.loadTasks();
        this.nextId = tasks.isEmpty() ? 1 : tasks.get(tasks.size() - 1).getId() + 1;
    }

    @Override
    public void addTask(String description) {
        Task task = new Task(nextId, description, TaskStatus.TODO);
        tasks.add(task);
        taskRepository.saveTasks(tasks);
        System.out.println("Task added successfully (ID: " + nextId + ")");
    }

    @Override
    public void updateTask(int id, String descripion) {
        for(Task task : tasks) {
            if(task.getId() == id) {
                task.setDescription(descripion);
                taskRepository.saveTasks(tasks);
                System.out.println("Task added successfully (ID: " + id + ")");
                return;
            }
        }
        System.out.println("Task with ID " + id + " not found.");
    }

    @Override
    public void deleteTask(int id) {
        for(Task task : tasks) {
            if(task.getId() == id) {
                tasks.remove(task);
                taskRepository.saveTasks(tasks);
                System.out.println("Task deleted successfully (ID: " + id + ")");
                return;
            }
        }
        System.out.println("Task with ID " + id + " not found.");

    }

    @Override
    public void markInProgressTask(int id) {
        for(Task task : tasks) {
            if(task.getId() == id) {
                task.setStatus(TaskStatus.IN_PROGRESS);
                taskRepository.saveTasks(tasks);
                System.out.println("Task marked as IN_PROGRESS successfully (ID: " + id + ")");
                return;
            }
        }
        System.out.println("Task with ID " + id + " not found.");

    }

    @Override
    public void markDoneTask(int id) {
        for(Task task : tasks) {
            if(task.getId() == id) {
                task.setStatus(TaskStatus.DONE);
                taskRepository.saveTasks(tasks);
                System.out.println("Task marked as DONE successfully (ID: " + id + ")");
                return;
            }
        }
        System.out.println("Task with ID " + id + " not found.");

    }

    @Override
    public List<Task> listAllTasks() {
        return tasks;
    }

    @Override
    public List<Task> listTaskByStatus(TaskStatus status) {
        return tasks.stream().filter(task -> task.getStatus() == status).collect(Collectors.toList());
    }

    @Override
    public void printAllTasks(List<Task> tasks) {
        if(tasks.isEmpty()) {
            System.out.println("No Tasks Found");
        } else {
            for(Task task : tasks) {
                System.out.println(task);
            }
        }

    }

}

```

Son olarak; CLI’da çalışan bir uygulama istendiği için CLI’dan gelen argümanları çözüp, fonksiyonları gerçekleştirecek main class’ını yazmalıyız. **cli** altında **TaskTracker** class’ını oluşturalım. switch-case kullanarak istenen işlemleri gerçekleştirelim.

_TaskTracker.java_

```java
package cli;

import model.Task;
import model.TaskStatus;
import repository.TaskRepository;
import repository.TaskRepositoryImpl;
import service.TaskService;
import service.TaskServiceImpl;

import java.util.List;

public class TaskTracker {
    public static void main(String[] args) {
        TaskRepository taskRepository = new TaskRepositoryImpl();
        TaskService taskService = new TaskServiceImpl(taskRepository);

        if (args.length == 0) {
            System.out.println("Usage: task-cli <command> [arguments]");
            return;
        }
        String command = args[0];

        switch (command) {
            case "add":
                if (args.length < 2) {
                    System.out.println("Usage: task-cli add <description>");
                }
                String description = args[1];
                taskService.addTask(description);
                break;

            case "update":
                if (args.length < 3) {
                    System.out.println("Usage: task-cli update <id> <description>");
                }
                int updateId = Integer.parseInt(args[1]);
                String newDescription = args[2];
                taskService.updateTask(updateId, newDescription);
                break;
            case "delete":
                if (args.length < 2) {
                    System.out.println("Usage: task-cli delete <id>");
                }
                int deleteId = Integer.parseInt(args[1]);
                taskService.deleteTask(deleteId);
                break;
            case "mark-in-progress":
                if (args.length < 2) {
                    System.out.println("Usage task-cli mark-in-progress <id>");
                }
                int inProgressId = Integer.parseInt(args[1]);
                taskService.markInProgressTask(inProgressId);
                break;
            case "mark-done":
                if (args.length < 2) {
                    System.out.println("Usage task-cli mark-done <id>");
                }
                int doneId = Integer.parseInt(args[1]);
                taskService.markDoneTask(doneId);
                break;
            case "list":
                if (args.length == 1) {
                    List<Task> allTasks = taskService.listAllTasks();
                    taskService.printAllTasks(allTasks);
                } else if (args.length == 2) {
                    String statusArg = args[1].toUpperCase().replace("-", "_");
                    try {
                        TaskStatus status = TaskStatus.valueOf(statusArg);
                        List<Task> filteredTasks = taskService.listTaskByStatus(status);
                        taskService.printAllTasks(filteredTasks);
                    } catch (IllegalArgumentException e) {
                        System.out.println("Invalid status. Available statuses: TODO, IN_PROGRESS, DONE");
                        e.printStackTrace();
                    }
                } else {
                    System.out.println("Usage: task-cli list [status]");
                }
                break;
            default:
                System.out.println("Invalid command. Available commands: add, update, delete, mark-in-progress, mark-done, list");
                break;
        }
    }
}

```