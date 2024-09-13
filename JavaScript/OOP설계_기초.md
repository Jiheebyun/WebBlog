<details>
  <summary>OOP Todo List 설계 </summary>

#### 요구 사항:
#### 클래스 설계:

### Task: 할 일(To-Do) 항목을 나타내는 클래스를 만드세요.
#### 속성: title: 할 일의 제목 (문자열)
- completed: 할 일이 완료되었는지 여부 (불리언, 기본값은 false)
- 메서드: toggle(): 할 일의 완료 상태를 반전시키는 메서드 (true ↔ false).
### TaskList: 할 일 목록을 관리하는 클래스를 만드세요.
#### 속성: tasks: Task 객체의 배열
- 메서드:addTask(title): 새로운 할 일을 추가하는 메서드.
- removeTask(index): 특정 인덱스에 있는 할 일을 삭제하는 메서드.
- getTasks(): 모든 할 일 목록을 반환하는 메서드.
- getCompletedTasks(): 완료된 할 일 목록을 반환하는 메서드.
- getPendingTasks(): 완료되지 않은 할 일 목록을 반환하는 메서드.

#### DOM 조작:
1. 사용자가 할 일을 추가할 수 있는 입력 필드와 버튼을 만드세요.
2. 할 일 목록이 화면에 표시되도록 하세요.
3. 각 할 일 항목 옆에는 완료 여부를 토글하는 버튼과 삭제 버튼이 있어야 합니다.
4. 할 일이 완료되면 항목의 스타일이 변경되도록 하세요(예: 텍스트에 취소선 추가).

```javascript
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        .todo-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 5px;
            border-bottom: 1px solid #ccc;
        }
        .todo-item.completed .title {
            text-decoration: line-through;
            color: #888;
        }
        button {
            margin-left: 5px;
        }
    </style>
</head>
<body>
    <h1>To-Do List</h1>

    <input type="text" id="new-task" placeholder="새 할 일을 입력하세요">
    <button id="add-task">추가</button>

    <h2>할 일 목록</h2>
    <div id="task-list"></div>

    <script>
        class Task {
            constructor(title) {
                this.title = title;
                this.completed = false;
            }

            toggle() {
                this.completed = !this.completed;
            }
        }

        // TaskList 클래스
        class TaskList {
            constructor() {
                this.tasks = [];
            }

            addTask(title) {
                const task = new Task(title);
                this.tasks.push(task);
                this.render();  
            }

            removeTask(index) {
                this.tasks.splice(index, 1);
                this.render(); 
            }

            toggleTask(index) {
                this.tasks[index].toggle();
                this.render(); 
            }

            render() {
                const taskListElement = document.getElementById('task-list');
                taskListElement.innerHTML = '';  // 기존 내용 초기화

                this.tasks.forEach((task, index) => {
                    const taskItem = document.createElement('div');
                    taskItem.className = 'todo-item';
                    if (task.completed) {
                        taskItem.classList.add('completed');
                    }

                    const titleElement = document.createElement('span');
                    titleElement.className = 'title';
                    titleElement.textContent = task.title;

                    const toggleButton = document.createElement('button');
                    toggleButton.textContent = '완료';
                    toggleButton.onclick = () => this.toggleTask(index);

                    const deleteButton = document.createElement('button');
                    deleteButton.textContent = '삭제';
                    deleteButton.onclick = () => this.removeTask(index);

                    taskItem.appendChild(titleElement);
                    taskItem.appendChild(toggleButton);
                    taskItem.appendChild(deleteButton);

                    taskListElement.appendChild(taskItem);
                });
            }
        }

        const taskList = new TaskList();

        document.getElementById('add-task').addEventListener('click', () => {
            const taskTitle = document.getElementById('new-task').value;
            if (taskTitle) {
                taskList.addTask(taskTitle);
                document.getElementById('new-task').value = '';  // 입력 필드 초기화
            }
        });

    </script>
</body>
</html>


```





</details>
