---
layout: post
title: Whither Components
date: "2023-07-23 10:30"
description: I don't know what this is yet, but it's about Components.
---

I don't know what this is yet, but it's about Components.

I have a number of goals:
1. Come up with a definition of "Component" that is all-encompassing, widely accepted, and succinct.
1. Write the "ideal Component", which is code that simply looks the most like what I think a TODO List Component's code should look like.
1. Make specs for:
   1. The shortest possible TODO List, by LOC.
   1. The most comprehensive TODO List, in terms of Configurability.

### Preamble

All Components should be:
1. Reusable
1. Composable
1. Modular
1. Focused
1. Encapsulation...ed

Components tend to have the following goals:
1. Represent a visible element of the page.
1. Bundle or generate chunks of HTML, CSS, and JS.
1. Expose configuration properties and hide local state.

Some common features seen in Components:
1. Lifecycle Methods (onMount, onUpdate, onUnmount, etc)
1. HTML Templates (Handlebars, JSX, Markdown, etc)
1. State Management (State Machines, Reducers, Streams, etc)
1. Immutable Objects (Immutable.js, Immer, SolidJS, etc)
1. Data-Fetching Libraries (GraphQL, React-Query, etc)

There are, broadly speaking, two kinds of Components:
1. Functional Components
1. Class Components

Purely Functional Components are far easier to write, because all you have to think about is Input and Output. Introducing Classes and Object-Oriented Programming techniques multiplies complexity, because it introduces Side-Effects. However, OOP is also easier to understand, as it more accurately mirrors our model of the world around us.

Here's an exhaustive list of Component-based Frameworks and a TODO List Component written in each.

---

### Angular
```typescript
// todo-list.component.ts
import { Component } from '@angular/core';

interface Task {
  id: number;
  title: string;
}

@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
  styleUrls: ['./todo-list.component.css'],
})
export class TodoListComponent {
  tasks: Task[] = [];
  newTask: string = '';

  addTask() {
    if (this.newTask.trim() === '') {
      return;
    }

    const task: Task = {
      id: Date.now(), // Simple way to generate a unique ID for the task
      title: this.newTask.trim(),
    };

    this.tasks.push(task);
    this.newTask = '';
  }

  removeTask(id: number) {
    this.tasks = this.tasks.filter((task) => task.id !== id);
  }
}
```

```html
<!-- todo-list.component.html -->
<div class="todo-container">
  <div class="input-container">
    <input
      type="text"
      [(ngModel)]="newTask"
      (keyup.enter)="addTask()"
      placeholder="Add a new task..."
    />
    <button (click)="addTask()">Add</button>
  </div>

  <ul class="task-list">
    <li *ngFor="let task of tasks">
      {{ task.title }}
      <button (click)="removeTask(task.id)">Remove</button>
    </li>
  </ul>
</div>
```

```html
<!-- app.component.html -->
<div class="app-container">
  <app-todo-list></app-todo-list>
</div>
```

---

### Aurelia
```javascript
// src/todo-list/todo-list.ts
export class TodoList {
  tasks = [];
  newTask = '';

  addTask() {
    if (this.newTask.trim() === '') {
      return;
    }

    const task = {
      id: Date.now(), // Simple way to generate a unique ID for the task
      title: this.newTask.trim(),
    };

    this.tasks.push(task);
    this.newTask = '';
  }

  removeTask(id) {
    this.tasks = this.tasks.filter((task) => task.id !== id);
  }
}
```

```html
<!-- src/todo-list/todo-list.html -->
<template>
  <div class="todo-container">
    <div class="input-container">
      <input
        type="text"
        value.bind="newTask"
        keyup.trigger="addTask($event)"
        placeholder="Add a new task..."
      />
      <button click.trigger="addTask()">Add</button>
    </div>

    <ul class="task-list">
      <li repeat.for="task of tasks">
        ${task.title}
        <button click.trigger="removeTask(task.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>
```

```html
<!-- src/app.html -->
<template>
  <require from="./todo-list/todo-list"></require>
  <todo-list></todo-list>
</template>
```

---

### Backbone
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
import { View, Model, Collection } from 'backbone';

// Task Model
const TaskModel = Model.extend({});

// Task Collection
const TaskCollection = Collection.extend({
  model: TaskModel,
});

// TODO List View
const TodoListView = View.extend({
  el: '#app',

  events: {
    'click #addTask': 'addTask',
    'click .remove': 'removeTask',
  },

  initialize() {
    this.tasks = new TaskCollection();
    this.render();
  },

  render() {
    const template = `
      <input type="text" id="taskTitle">
      <button id="addTask">Add</button>
      <ul id="taskList">
        <% tasks.forEach(function(task) { %>
          <li><%= task.get('title') %> <button class="remove" data-id="<%= task.cid %>">Remove</button></li>
        <% }); %>
      </ul>
    `;
    this.$el.html(_.template(template)({ tasks: this.tasks.models }));
  },

  addTask() {
    const title = this.$('#taskTitle').val().trim();
    if (title) {
      this.tasks.add({ title });
      this.render();
      this.$('#taskTitle').val('');
    }
  },

  removeTask(event) {
    const taskId = event.target.getAttribute('data-id');
    const taskToRemove = this.tasks.get(taskId);
    if (taskToRemove) {
      this.tasks.remove(taskToRemove);
      this.render();
    }
  },
});

new TodoListView();
```

---

### Blazor
```razor
<!-- TodoListApp/Pages/Index.razor -->
@page "/"
<h3>TODO List</h3>

<div class="input-group">
    <input type="text" class="form-control" @bind="newTask" />
    <div class="input-group-append">
        <button class="btn btn-outline-secondary" @onclick="AddTask">Add</button>
    </div>
</div>

<ul class="list-group mt-3">
    @foreach (var task in tasks)
    {
        <li class="list-group-item d-flex justify-content-between">
            @task
            <button class="btn btn-danger btn-sm" @onclick="() => RemoveTask(task)">Remove</button>
        </li>
    }
</ul>

@code {
    private List<string> tasks = new List<string>();
    private string newTask = "";

    private void AddTask()
    {
        if (!string.IsNullOrWhiteSpace(newTask))
        {
            tasks.Add(newTask);
            newTask = "";
        }
    }

    private void RemoveTask(string task)
    {
        tasks.Remove(task);
    }
}
```

---

### Cycle
```javascript
// app.js
import { run } from '@cycle/xstream-run';
import { makeDOMDriver, div, input, button, ul, li } from '@cycle/dom';

function main(sources) {
  // Intent: Capture user actions
  const addTaskAction$ = sources.DOM.select('.add-task').events('click');
  const newTaskInput$ = sources.DOM.select('.new-task').events('input');

  // Model: Handle state changes
  const newTaskReducer$ = newTaskInput$
    .map((ev) => ev.target.value)
    .startWith('')
    .map((newTask) => (state) => ({ ...state, newTask }));

  const addTaskReducer$ = addTaskAction$
    .map(() => (state) => {
      if (state.newTask.trim() === '') {
        return state;
      }

      const newTask = state.newTask.trim();
      const tasks = state.tasks.concat(newTask);
      return { ...state, tasks, newTask: '' };
    });

  const reducer$ = xstream.merge(newTaskReducer$, addTaskReducer$);

  // Model: Initial state
  const initialState = {
    tasks: [],
    newTask: '',
  };

  // Model: Accumulate state changes
  const state$ = reducer$.fold((state, reducer) => reducer(state), initialState);

  // View: Render the UI
  const vdom$ = state$.map((state) =>
    div('.todo-container', [
      input('.new-task', {
        attrs: { type: 'text', placeholder: 'Add a new task...', value: state.newTask },
      }),
      button('.add-task', 'Add'),
      ul('.task-list', state.tasks.map((task) => li(task))),
    ])
  );

  return {
    DOM: vdom$,
  };
}

const drivers = {
  DOM: makeDOMDriver('#app'),
};

run(main, drivers);
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

### Dojo
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script>
    // Configure Dojo Toolkit
    var dojoConfig = {
      async: true,
    };
  </script>
  <script src="node_modules/dojo/dojo.js"></script>
  <script>
    // Load the main application module
    require(['app']);
  </script>
</body>
</html>
```

```javascript
// app.js
require([
  'dojo/dom',
  'dojo/dom-construct',
  'dojo/query',
  'dojo/on',
  'dojo/domReady!',
], function (dom, domConstruct, query, on) {
  // Task data
  var tasks = [];

  // Function to render the list of tasks
  function renderTasks() {
    var taskList = dom.byId('taskList');
    domConstruct.empty(taskList);

    tasks.forEach(function (task, index) {
      var taskItem = domConstruct.create('li', {
        innerHTML: task.title + ' <button class="remove" data-index="' + index + '">Remove</button>',
      });
      domConstruct.place(taskItem, taskList);
    });
  }

  // Function to add a new task
  function addTask(newTask) {
    if (newTask.trim() !== '') {
      tasks.push({ title: newTask.trim() });
      renderTasks();
    }
  }

  // Function to remove a task
  function removeTask(index) {
    tasks.splice(index, 1);
    renderTasks();
  }

  // DOM elements and event handlers
  var newTaskInput = dom.byId('newTask');
  var addTaskButton = dom.byId('addTask');
  var taskList = dom.byId('taskList');

  on(addTaskButton, 'click', function () {
    addTask(newTaskInput.value);
    newTaskInput.value = '';
  });

  on(taskList, '.remove:click', function (event) {
    var index = event.target.getAttribute('data-index');
    removeTask(index);
  });
});
```

```html
<!-- app.html -->
<div class="todo-container">
  <input type="text" id="newTask" placeholder="Add a new task...">
  <button id="addTask">Add</button>
  <ul id="taskList"></ul>
</div>
```

---

### Ember
```handlebars
<!-- app/templates/components/todo-list.hbs -->
<div class="todo-container">
  {{input type="text" value=newTask placeholder="Add a new task..." action=(action "addTask")}}
  <button {{action "addTask"}}>Add</button>
  <ul class="task-list">
    {{#each tasks as |task|}}
      <li>{{task}} <button {{action "removeTask" task}}>Remove</button></li>
    {{/each}}
  </ul>
</div>
```

```javascript
// app/components/todo-list.js
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';

export default class TodoListComponent extends Component {
  @tracked tasks = [];
  @tracked newTask = '';

  addTask() {
    if (this.newTask.trim() !== '') {
      this.tasks.push(this.newTask.trim());
      this.newTask = '';
    }
  }

  removeTask(task) {
    this.tasks.removeObject(task);
  }
}
```

```handlebars
<!-- app/templates/application.hbs -->
<div class="app-container">
  <TodoList />
</div>
```

---

### HTML Components
```html
<!-- todo-list.htc -->
<SCRIPT LANGUAGE="javascript">
  // Initialize tasks array
  tasks = [];

  // Add task function
  function addTask() {
    var newTaskInput = document.getElementById('newTask');
    var taskList = document.getElementById('taskList');
    var newTask = newTaskInput.value.trim();
    if (newTask !== '') {
      tasks.push(newTask);
      newTaskInput.value = '';
      renderTasks();
    }
  }

  // Remove task function
  function removeTask(index) {
    tasks.splice(index, 1);
    renderTasks();
  }

  // Render tasks function
  function renderTasks() {
    var taskList = document.getElementById('taskList');
    taskList.innerHTML = '';

    for (var i = 0; i < tasks.length; i++) {
      var listItem = document.createElement('li');
      listItem.innerHTML = tasks[i] + ' <button onclick="removeTask(' + i + ')">Remove</button>';
      taskList.appendChild(listItem);
    }
  }
</SCRIPT>

<DIV CLASS="todo-container">
  <INPUT TYPE="text" ID="newTask" VALUE="" PLACEHOLDER="Add a new task...">
  <BUTTON LANGUAGE="javascript" ONCLICK="addTask()">Add</BUTTON>
  <UL ID="taskList"></UL>
</DIV>
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app">
    <!-- Use the HTML Component -->
    <object classid="clsid:333C7BC4-460F-11D0-BC04-0080C7055A83" data="todo-list.htc"></object>
  </div>
</body>
</html>
```

---

### Hyperapp
```javascript
// app.js
import { h, app } from "hyperapp";

const state = {
  tasks: [],
  newTask: "",
};

const actions = {
  updateNewTask: (value) => (state) => ({ ...state, newTask: value }),
  addTask: () => (state) => {
    if (state.newTask.trim() !== "") {
      const newTask = state.newTask.trim();
      return {
        ...state,
        tasks: state.tasks.concat(newTask),
        newTask: "",
      };
    }
    return state;
  },
  removeTask: (index) => (state) => ({
    ...state,
    tasks: state.tasks.filter((_, i) => i !== index),
  }),
};

const view = (state, actions) => (
  <div class="todo-container">
    <input
      type="text"
      value={state.newTask}
      oninput={(e) => actions.updateNewTask(e.target.value)}
      placeholder="Add a new task..."
    />
    <button onclick={actions.addTask}>Add</button>
    <ul class="task-list">
      {state.tasks.map((task, index) => (
        <li>
          {task} <button onclick={() => actions.removeTask(index)}>Remove</button>
        </li>
      ))}
    </ul>
  </div>
);

const main = app(state, actions, view, document.getElementById("app"));
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

### Inferno
```javascript
// app.js
import Inferno from "inferno";
import { render } from "inferno";
import createElement from "inferno-create-element";

const state = {
  tasks: [],
  newTask: "",
};

const actions = {
  updateNewTask: (value) => (state) => ({ ...state, newTask: value }),
  addTask: () => (state) => {
    if (state.newTask.trim() !== "") {
      const newTask = state.newTask.trim();
      return {
        ...state,
        tasks: state.tasks.concat(newTask),
        newTask: "",
      };
    }
    return state;
  },
  removeTask: (index) => (state) => ({
    ...state,
    tasks: state.tasks.filter((_, i) => i !== index),
  }),
};

const TodoList = ({ state, actions }) => (
  <div class="todo-container">
    <input
      type="text"
      value={state.newTask}
      onInput={(e) => actions.updateNewTask(e.target.value)}
      placeholder="Add a new task..."
    />
    <button onClick={actions.addTask}>Add</button>
    <ul class="task-list">
      {state.tasks.map((task, index) => (
        <li>
          {task} <button onClick={() => actions.removeTask(index)}>Remove</button>
        </li>
      ))}
    </ul>
  </div>
);

render(<TodoList state={state} actions={actions} />, document.getElementById("app"));
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

### Knockout
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app">
    <div class="todo-container">
      <input type="text" data-bind="value: newTask, valueUpdate: 'afterkeydown'" placeholder="Add a new task...">
      <button data-bind="click: addTask">Add</button>
      <ul class="task-list" data-bind="foreach: tasks">
        <li>
          <span data-bind="text: $data"></span>
          <button data-bind="click: $parent.removeTask">Remove</button>
        </li>
      </ul>
    </div>
  </div>

  <!-- Include Knockout.js library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/knockout/3.5.1/knockout-min.js"></script>
  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
function TodoListViewModel() {
  var self = this;
  self.tasks = ko.observableArray([]);
  self.newTask = ko.observable("");

  self.addTask = function() {
    var newTask = self.newTask().trim();
    if (newTask !== "") {
      self.tasks.push(newTask);
      self.newTask("");
    }
  };

  self.removeTask = function(task) {
    self.tasks.remove(task);
  };
}

// Apply bindings
ko.applyBindings(new TodoListViewModel(), document.getElementById("app"));
```

---

### Lit
```javascript
// todo-list.js
import { LitElement, html, css } from 'lit';

class TodoList extends LitElement {
  static styles = css`
    .todo-container {
      margin-top: 20px;
    }

    .task-list li {
      margin-bottom: 5px;
    }
  `;

  static properties = {
    tasks: { type: Array },
    newTask: { type: String },
  };

  constructor() {
    super();
    this.tasks = [];
    this.newTask = '';
  }

  updateNewTask(e) {
    this.newTask = e.target.value;
  }

  addTask() {
    if (this.newTask.trim() !== '') {
      const newTask = this.newTask.trim();
      this.tasks = [...this.tasks, newTask];
      this.newTask = '';
    }
  }

  removeTask(index) {
    this.tasks = this.tasks.filter((_, i) => i !== index);
  }

  render() {
    return html`
      <div class="todo-container">
        <input
          type="text"
          .value="${this.newTask}"
          @input="${this.updateNewTask}"
          placeholder="Add a new task..."
        />
        <button @click="${this.addTask}">Add</button>
        <ul class="task-list">
          ${this.tasks.map(
            (task, index) => html`
              <li>
                ${task} <button @click="${() => this.removeTask(index)}">Remove</button>
              </li>
            `
          )}
        </ul>
      </div>
    `;
  }
}

customElements.define('todo-list', TodoList);
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app">
    <todo-list></todo-list>
  </div>

  <script type="module" src="todo-list.js"></script>
</body>
</html>
```

---

### Marko
```marko
<!-- todo-list.marko -->
class {
  onCreate() {
    this.state = {
      tasks: [],
      newTask: '',
    };
  }

  updateNewTask(event) {
    this.state.newTask = event.target.value;
  }

  addTask() {
    const newTask = this.state.newTask.trim();
    if (newTask !== '') {
      this.state.tasks.push(newTask);
      this.state.newTask = '';
    }
  }

  removeTask(index) {
    this.state.tasks.splice(index, 1);
  }
}

<div class="todo-container">
  <input
    type="text"
    value(this.state.newTask)
    on-input('updateNewTask')
    placeholder="Add a new task..."
  />
  <button on-click('addTask')>Add</button>
  <ul class="task-list">
    <for each="task, index in state.tasks">
      <li>
        ${task} <button on-click('removeTask', index)>Remove</button>
      </li>
    </for>
  </ul>
</div>
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
const template = require('./todo-list.marko');

template.render({}, document.getElementById('app'));
```

---

### Meteor
```html
<!-- client/todoList.html -->
<template name="todoList">
  <div class="todo-container">
    <input type="text" placeholder="Add a new task..." id="newTask">
    <button class="add-task">Add</button>
    <ul class="task-list">
      {{#each tasks}}
        <li>{{this}} <button class="remove-task">Remove</button></li>
      {{/each}}
    </ul>
  </div>
</template>
```

```javascript
// client/todoList.js
import { Template } from 'meteor/templating';

Template.todoList.onCreated(function() {
  this.tasks = new ReactiveVar([]);
});

Template.todoList.helpers({
  tasks() {
    const instance = Template.instance();
    return instance.tasks.get();
  },
});

Template.todoList.events({
  'click .add-task'(event, instance) {
    const newTaskInput = instance.find('#newTask');
    const newTask = newTaskInput.value.trim();
    if (newTask !== '') {
      instance.tasks.set([...instance.tasks.get(), newTask]);
      newTaskInput.value = '';
    }
  },
  'click .remove-task'(event, instance) {
    const index = $(event.target).closest('li').index();
    const tasks = instance.tasks.get();
    tasks.splice(index, 1);
    instance.tasks.set(tasks);
  },
});
```

```html
<!-- client/main.html -->
<head>
  <title>TODO List</title>
</head>

<body>
  <div id="app">
    {{> todoList}}
  </div>
</body>
```

---

### Mithril
```javascript
// app.js
import m from "mithril";

const TodoList = {
  tasks: [],
  newTask: "",

  addTask: () => {
    if (TodoList.newTask.trim() !== "") {
      const newTask = TodoList.newTask.trim();
      TodoList.tasks.push(newTask);
      TodoList.newTask = "";
    }
  },

  removeTask: (index) => {
    TodoList.tasks.splice(index, 1);
  },

  view: () => {
    return m(".todo-container", [
      m("input[type=text]", {
        value: TodoList.newTask,
        oninput: (e) => (TodoList.newTask = e.target.value),
        placeholder: "Add a new task...",
      }),
      m("button", { onclick: TodoList.addTask }, "Add"),
      m("ul.task-list", TodoList.tasks.map((task, index) =>
        m("li", [
          task,
          m("button", { onclick: () => TodoList.removeTask(index) }, "Remove"),
        ])
      )),
    ]);
  },
};

m.mount(document.getElementById("app"), TodoList);
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

### Polymer
```javascript
// src/todo-list-app.js
import { PolymerElement, html } from '@polymer/polymer/polymer-element.js';

class TodoListApp extends PolymerElement {
  static get properties() {
    return {
      tasks: {
        type: Array,
        value: () => [],
      },
      newTask: {
        type: String,
        value: '',
      },
    };
  }

  _addTask() {
    if (this.newTask.trim() !== '') {
      const newTask = this.newTask.trim();
      this.push('tasks', newTask);
      this.newTask = '';
    }
  }

  _removeTask(e) {
    const index = e.model.index;
    this.splice('tasks', index, 1);
  }

  static get template() {
    return html`
      <style>
        .todo-container {
          margin-top: 20px;
        }

        .task-list li {
          margin-bottom: 5px;
        }
      </style>

      <div class="todo-container">
        <input
          type="text"
          value="{{newTask::input}}"
          placeholder="Add a new task..."
        />
        <button on-click="_addTask">Add</button>
        <ul class="task-list">
          <template is="dom-repeat" items="{{tasks}}" as="task" index-as="index">
            <li>
              {{task}} <button on-click="_removeTask">Remove</button>
            </li>
          </template>
        </ul>
      </div>
    `;
  }
}

customElements.define('todo-list-app', TodoListApp);
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <todo-list-app></todo-list-app>

  <script type="module" src="./src/todo-list-app.js"></script>
</body>
</html>
```

---

### Preact
```javascript
// app.js
import { h, render } from 'preact';

const TodoList = () => {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  const addTask = () => {
    if (newTask.trim() !== '') {
      const updatedTasks = [...tasks, newTask.trim()];
      setTasks(updatedTasks);
      setNewTask('');
    }
  };

  const removeTask = (index) => {
    const updatedTasks = tasks.filter((_, i) => i !== index);
    setTasks(updatedTasks);
  };

  return (
    <div class="todo-container">
      <input
        type="text"
        value={newTask}
        onInput={(e) => setNewTask(e.target.value)}
        placeholder="Add a new task..."
      />
      <button onClick={addTask}>Add</button>
      <ul class="task-list">
        {tasks.map((task, index) => (
          <li>
            {task} <button onClick={() => removeTask(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

render(<TodoList />, document.getElementById('app'));
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

### Ractive
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
import Ractive from 'ractive';

const TodoList = new Ractive({
  el: '#app',
  template: `
    <div class="todo-container">
      <input type="text" value="{{newTask}}" placeholder="Add a new task...">
      <button on-click="addTask">Add</button>
      <ul class="task-list">
        {{#each tasks:i}}
          <li>
            {{.}} <button on-click="removeTask(i)">Remove</button>
          </li>
        {{/each}}
      </ul>
    </div>
  `,
  data: {
    tasks: [],
    newTask: '',
  },
  addTask: function() {
    if (this.get('newTask').trim() !== '') {
      const newTask = this.get('newTask').trim();
      this.push('tasks', newTask);
      this.set('newTask', '');
    }
  },
  removeTask: function(index) {
    this.splice('tasks', index, 1);
  },
});

export default TodoList;
```

---

### React
```javascript
// src/App.js
import React, { useState } from 'react';
import './App.css';

function App() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  const addTask = () => {
    if (newTask.trim() !== '') {
      const updatedTasks = [...tasks, newTask.trim()];
      setTasks(updatedTasks);
      setNewTask('');
    }
  };

  const removeTask = (index) => {
    const updatedTasks = tasks.filter((_, i) => i !== index);
    setTasks(updatedTasks);
  };

  return (
    <div className="todo-container">
      <input
        type="text"
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
        placeholder="Add a new task..."
      />
      <button onClick={addTask}>Add</button>
      <ul className="task-list">
        {tasks.map((task, index) => (
          <li key={index}>
            {task} <button onClick={() => removeTask(index)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

```css
/* src/App.css */
.todo-container {
  margin-top: 20px;
}

.task-list li {
  margin-bottom: 5px;
}
```

---

### Riot
```html
<!-- todo-list.riot -->
<todo-list>
  <div class="todo-container">
    <input type="text" value={newTask} oninput={updateNewTask} placeholder="Add a new task...">
    <button onclick={addTask}>Add</button>
    <ul class="task-list">
      <li each={task, index in tasks}>
        {task} <button onclick={removeTask(index)}>Remove</button>
      </li>
    </ul>
  </div>

  <script>
    export default {
      tasks: [],
      newTask: '',

      updateNewTask(event) {
        this.newTask = event.target.value;
      },

      addTask() {
        if (this.newTask.trim() !== '') {
          const newTask = this.newTask.trim();
          this.tasks.push(newTask);
          this.newTask = '';
        }
      },

      removeTask(index) {
        this.tasks.splice(index, 1);
      },
    };
  </script>

  <style>
    .todo-container {
      margin-top: 20px;
    }

    .task-list li {
      margin-bottom: 5px;
    }
  </style>
</todo-list>
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <todo-list></todo-list>

  <script src="riot.min.js"></script>
  <script src="todo-list.riot"></script>
</body>
</html>
```

---

### Stencil
```tsx
// src/components/todo-list/todo-list.tsx
import { Component, h, State } from '@stencil/core';

@Component({
  tag: 'todo-list',
  styleUrl: 'todo-list.css',
  shadow: true,
})
export class TodoList {
  @State() tasks: string[] = [];
  @State() newTask: string = '';

  updateNewTask(event: any) {
    this.newTask = event.target.value;
  }

  addTask() {
    if (this.newTask.trim() !== '') {
      const newTask = this.newTask.trim();
      this.tasks = [...this.tasks, newTask];
      this.newTask = '';
    }
  }

  removeTask(index: number) {
    this.tasks = this.tasks.filter((_, i) => i !== index);
  }

  render() {
    return (
      <div class="todo-container">
        <input
          type="text"
          value={this.newTask}
          onInput={(e: any) => this.updateNewTask(e)}
          placeholder="Add a new task..."
        />
        <button onClick={() => this.addTask()}>Add</button>
        <ul class="task-list">
          {this.tasks.map((task, index) => (
            <li>
              {task} <button onClick={() => this.removeTask(index)}>Remove</button>
            </li>
          ))}
        </ul>
      </div>
    );
  }
}
```

```css
/* src/components/todo-list/todo-list.css */
.todo-container {
  margin-top: 20px;
}

.task-list li {
  margin-bottom: 5px;
}
```

---

### Stimulus
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <div data-controller="todo-list">
    <div class="todo-container">
      <input type="text" data-target="todo-list.newTask" placeholder="Add a new task...">
      <button data-action="click->todo-list#addTask">Add</button>
      <ul class="task-list" data-target="todo-list.taskList">
        <li data-target="todo-list.task" data-action="click->todo-list#removeTask">
          {{task}} <button>Remove</button>
        </li>
      </ul>
    </div>
  </div>

  <script src="stimulus.umd.min.js"></script>
  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
import { Controller } from 'stimulus';

export default class extends Controller {
  static targets = ['newTask', 'taskList', 'task'];

  initialize() {
    this.tasks = [];
  }

  addTask() {
    const newTask = this.newTaskTarget.value.trim();
    if (newTask !== '') {
      this.tasks.push(newTask);
      this.newTaskTarget.value = '';
      this.renderTasks();
    }
  }

  removeTask(event) {
    const index = this.taskTargets.indexOf(event.target.closest('li'));
    if (index !== -1) {
      this.tasks.splice(index, 1);
      this.renderTasks();
    }
  }

  renderTasks() {
    this.taskListTarget.innerHTML = '';
    this.tasks.forEach((task) => {
      const li = document.createElement('li');
      li.textContent = task;
      const button = document.createElement('button');
      button.textContent = 'Remove';
      button.addEventListener('click', this.removeTask.bind(this));
      li.appendChild(button);
      this.taskListTarget.appendChild(li);
    });
  }
}
```

---

### Svelte
```html
<!-- src/App.svelte -->
<script>
  let tasks = [];
  let newTask = '';

  function addTask() {
    if (newTask.trim() !== '') {
      const updatedTasks = [...tasks, newTask.trim()];
      tasks = updatedTasks;
      newTask = '';
    }
  }

  function removeTask(index) {
    tasks = tasks.filter((_, i) => i !== index);
  }
</script>

<main>
  <div class="todo-container">
    <input type="text" bind:value={newTask} placeholder="Add a new task...">
    <button on:click={addTask}>Add</button>
    <ul class="task-list">
      {#each tasks as task, index}
        <li>
          {task} <button on:click={() => removeTask(index)}>Remove</button>
        </li>
      {/each}
    </ul>
  </div>
</main>

<style>
  .todo-container {
    margin-top: 20px;
  }

  .task-list li {
    margin-bottom: 5px;
  }
</style>
```

---

### Vue
```html
<!-- src/App.vue -->
<template>
  <div class="todo-container">
    <input type="text" v-model="newTask" placeholder="Add a new task...">
    <button @click="addTask">Add</button>
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index">
        {{ task }} <button @click="removeTask(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: '',
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        const newTask = this.newTask.trim();
        this.tasks.push(newTask);
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>

<style>
.todo-container {
  margin-top: 20px;
}

.task-list li {
  margin-bottom: 5px;
}
</style>
```

---

### Web Components
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
</head>
<body>
  <todo-list></todo-list>

  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
class TodoList extends HTMLElement {
  constructor() {
    super();

    this.tasks = [];
    this.newTask = '';

    const template = document.createElement('template');
    template.innerHTML = `
      <div class="todo-container">
        <input type="text" placeholder="Add a new task...">
        <button>Add</button>
        <ul class="task-list"></ul>
      </div>
    `;

    this.attachShadow({ mode: 'open' });
    this.shadowRoot.appendChild(template.content.cloneNode(true));

    this.input = this.shadowRoot.querySelector('input');
    this.button = this.shadowRoot.querySelector('button');
    this.taskList = this.shadowRoot.querySelector('.task-list');

    this.button.addEventListener('click', this.addTask.bind(this));
    this.input.addEventListener('input', this.updateNewTask.bind(this));
  }

  updateNewTask(event) {
    this.newTask = event.target.value;
  }

  addTask() {
    if (this.newTask.trim() !== '') {
      const newTask = this.newTask.trim();
      this.tasks.push(newTask);
      this.newTask = '';
      this.renderTasks();
    }
  }

  removeTask(index) {
    this.tasks.splice(index, 1);
    this.renderTasks();
  }

  renderTasks() {
    this.taskList.innerHTML = '';
    this.tasks.forEach((task, index) => {
      const li = document.createElement('li');
      li.textContent = task;
      const button = document.createElement('button');
      button.textContent = 'Remove';
      button.addEventListener('click', () => this.removeTask(index));
      li.appendChild(button);
      this.taskList.appendChild(li);
    });
  }

  connectedCallback() {
    this.renderTasks();
  }
}

customElements.define('todo-list', TodoList);
```

```html
<!-- index.html -->
<style>
  .todo-container {
    margin-top: 20px;
  }

  .task-list li {
    margin-bottom: 5px;
  }
</style>
```

---

### Webix
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TODO List</title>
  <link rel="stylesheet" href="https://cdn.webix.com/edge/webix.css" type="text/css">
  <script src="https://cdn.webix.com/edge/webix.js" type="text/javascript"></script>
</head>
<body>
  <div id="todo-list"></div>

  <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
webix.ready(() => {
  webix.ui({
    container: 'todo-list',
    rows: [
      {
        view: 'text',
        placeholder: 'Add a new task...',
        on: {
          onTimedKeyPress: function () {
            $$('taskList').add({ task: this.getValue() });
            this.setValue('');
          }
        }
      },
      {
        view: 'list',
        id: 'taskList',
        template: '#task# <span class="webix_icon wxi-close" onclick="removeTask(#id#)"></span>',
        data: []
      }
    ]
  });

  function removeTask(id) {
    $$('taskList').remove(id);
  }
});
```
