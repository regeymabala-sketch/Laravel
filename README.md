composer create-project laravel/laravel personal-task-manager
cd personal-task-manager

personal_task_manager

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=personal_task_manager
DB_USERNAME=root
DB_PASSWORD=

php artisan make:model Task -m

database/migrations/xxxx_xx_xx_xxxxxx_create_tasks_table.php

public function up(): void
{
    Schema::create('tasks', function (Blueprint $table) {
        $table->id();
        $table->string('task_name');
        $table->text('description')->nullable();
        $table->enum('status', ['Pending', 'Completed'])->default('Pending');
        $table->date('due_date')->nullable();
        $table->timestamps();
    });
}

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

php artisan migrate

app/Models/Task.php

<?php

<?php

namespace App\Http\Controllers;

use App\Models\Task;
use Illuminate\Http\Request;

class TaskController extends Controller
{
    /**
     * Display all tasks.
     */
    public function index()
    {
        $tasks = Task::orderBy('due_date', 'asc')->get();

        return view('tasks.index', compact('tasks'));
    }

    /**
     * Show the form for creating a new task.
     */
    public function create()
    {
        return view('tasks.create');
    }

    /**
     * Store a newly created task.
     */
    public function store(Request $request)
    {
        $request->validate([
            'task_name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'status' => 'required|in:Pending,Completed',
            'due_date' => 'nullable|date',
        ]);

        Task::create([
            'task_name' => $request->task_name,
            'description' => $request->description,
            'status' => $request->status,
            'due_date' => $request->due_date,
        ]);

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task added successfully!');
    }

    /**
     * Display a specific task.
     */
    public function show(Task $task)
    {
        return view('tasks.show', compact('task'));
    }

    /**
     * Show the form for editing a task.
     */
    public function edit(Task $task)
    {
        return view('tasks.edit', compact('task'));
    }

    /**
     * Update a task.
     */
    public function update(Request $request, Task $task)
    {
        $request->validate([
            'task_name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'status' => 'required|in:Pending,Completed',
            'due_date' => 'nullable|date',
        ]);

        $task->update([
            'task_name' => $request->task_name,
            'description' => $request->description,
            'status' => $request->status,
            'due_date' => $request->due_date,
        ]);

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task updated successfully!');
    }

    /**
     * Delete a task.
     */
    public function destroy(Task $task)
    {
        $task->delete();

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task deleted successfully!');
    }
}


namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Task extends Model
{
    protected $fillable = [
        'task_name',
        'description',
        'status',
        'due_date',
    ];

    protected $casts = [
        'due_date' => 'date',
    ];
}

php artisan make:controller TaskController --resource

app/Http/Controllers/TaskController.php

routes/web.php

<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\TaskController;

Route::get('/', function () {
    return redirect()->route('tasks.index');
});

Route::resource('tasks', TaskController::class);

resources/views/tasks

index.blade.php
create.blade.php
edit.blade.php
show.blade.php

resources/views/layouts

app.blade.php

resources
└── views
    ├── layouts
    │   └── app.blade.php
    │
    └── tasks
        ├── index.blade.php
        ├── create.blade.php
        ├── edit.blade.php
        └── show.blade.php

resources/views/layouts/app.blade.php

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Personal Task Manager</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            color: #333;
        }

        .navbar {
            background: #1f2937;
            color: white;
            padding: 18px 40px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .navbar h1 {
            font-size: 22px;
        }

        .container {
            width: 90%;
            max-width: 1100px;
            margin: 30px auto;
        }

        .card {
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }

        .page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .page-header h2 {
            font-size: 28px;
        }

        .btn {
            display: inline-block;
            padding: 9px 15px;
            border: none;
            border-radius: 6px;
            text-decoration: none;
            cursor: pointer;
            font-size: 14px;
        }

        .btn-primary {
            background: #2563eb;
            color: white;
        }

        .btn-success {
            background: #16a34a;
            color: white;
        }

        .btn-warning {
            background: #f59e0b;
            color: white;
        }

        .btn-danger {
            background: #dc2626;
            color: white;
        }

        .btn-secondary {
            background: #6b7280;
            color: white;
        }

        .btn:hover {
            opacity: 0.85;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 14px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background: #f3f4f6;
        }

        .status {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }

        .pending {
            background: #fef3c7;
            color: #92400e;
        }

        .completed {
            background: #dcfce7;
            color: #166534;
        }

        .alert {
            padding: 12px 15px;
            margin-bottom: 20px;
            border-radius: 6px;
            background: #dcfce7;
            color: #166534;
        }

        .form-group {
            margin-bottom: 18px;
        }

        label {
            display: block;
            margin-bottom: 7px;
            font-weight: bold;
        }

        input, textarea, select {
            width: 100%;
            padding: 11px;
            border: 1px solid #ccc;
            border-radius: 6px;
            font-size: 15px;
        }

        textarea {
            height: 120px;
            resize: vertical;
        }

        .error {
            color: #dc2626;
            font-size: 13px;
            margin-top: 5px;
        }

        .actions {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
        }

        .empty {
            text-align: center;
            padding: 40px;
            color: #777;
        }

        .task-details {
            line-height: 2;
        }

        @media (max-width: 768px) {
            .navbar {
                padding: 15px 20px;
            }

            .container {
                width: 95%;
            }

            table {
                font-size: 13px;
            }

            th, td {
                padding: 9px;
            }
        }
    </style>
</head>

<body>

    <nav class="navbar">
        <h1>Personal Task Manager</h1>

        <a href="{{ route('tasks.index') }}" class="btn btn-primary">
            All Tasks
        </a>
    </nav>

    <div class="container">

        @if(session('success'))
            <div class="alert">
                {{ session('success') }}
            </div>
        @endif

        @yield('content')

    </div>

</body>
</html>

resources/views/tasks/index.blade.php

@extends('layouts.app')

@section('content')

<div class="page-header">
    <h2>My Tasks</h2>

    <a href="{{ route('tasks.create') }}" class="btn btn-primary">
        + Add Task
    </a>
</div>

<div class="card">

    @if($tasks->count() > 0)

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Task Name</th>
                    <th>Description</th>
                    <th>Status</th>
                    <th>Due Date</th>
                    <th>Actions</th>
                </tr>
            </thead>

            <tbody>

                @foreach($tasks as $task)

                    <tr>
                        <td>{{ $task->id }}</td>

                        <td>
                            <strong>{{ $task->task_name }}</strong>
                        </td>

                        <td>
                            {{ Str::limit($task->description, 40) }}
                        </td>

                        <td>
                            @if($task->status == 'Completed')
                                <span class="status completed">
                                    Completed
                                </span>
                            @else
                                <span class="status pending">
                                    Pending
                                </span>
                            @endif
                        </td>

                        <td>
                            {{ $task->due_date ? $task->due_date->format('M d, Y') : 'No date' }}
                        </td>

                        <td>
                            <div class="actions">

                                <a href="{{ route('tasks.show', $task) }}"
                                   class="btn btn-success">
                                    View
                                </a>

                                <a href="{{ route('tasks.edit', $task) }}"
                                   class="btn btn-warning">
                                    Edit
                                </a>

                                <form action="{{ route('tasks.destroy', $task) }}"
                                      method="POST"
                                      onsubmit="return confirm('Are you sure you want to delete this task?');">

                                    @csrf
                                    @method('DELETE')

                                    <button type="submit"
                                            class="btn btn-danger">
                                        Delete
                                    </button>

                                </form>

                            </div>
                        </td>
                    </tr>

                @endforeach

            </tbody>
        </table>

    @else

        <div class="empty">
            <h3>No tasks found.</h3>
            <p>Click "Add Task" to create your first task.</p>
        </div>

    @endif

</div>

@endsection

resources/views/tasks/create.blade.php

@extends('layouts.app')

@section('content')

<div class="page-header">
    <h2>Add New Task</h2>

    <a href="{{ route('tasks.index') }}"
       class="btn btn-secondary">
        Back
    </a>
</div>

<div class="card">

    <form action="{{ route('tasks.store') }}" method="POST">

        @csrf

        <div class="form-group">
            <label for="task_name">Task Name</label>

            <input type="text"
                   id="task_name"
                   name="task_name"
                   value="{{ old('task_name') }}"
                   placeholder="Enter task name">

            @error('task_name')
                <div class="error">{{ $message }}</div>
            @enderror
        </div>

        <div class="form-group">
            <label for="description">Description</label>

            <textarea id="description"
                      name="description"
                      placeholder="Enter task details">{{ old('description') }}</textarea>

            @error('description')
                <div class="error">{{ $message }}</div>
            @enderror
        </div>

        <div class="form-group">
            <label for="status">Status</label>

            <select name="status" id="status">

                <option value="Pending"
                    {{ old('status') == 'Pending' ? 'selected' : '' }}>
                    Pending
                </option>

                <option value="Completed"
                    {{ old('status') == 'Completed' ? 'selected' : '' }}>
                    Completed
                </option>

            </select>

            @error('status')
                <div class="error">{{ $message }}</div>
            @enderror
        </div>

        <div class="form-group">
            <label for="due_date">Due Date</label>

            <input type="date"
                   id="due_date"
                   name="due_date"
                   value="{{ old('due_date') }}">

            @error('due_date')
                <div class="error">{{ $message }}</div>
            @enderror
        </div>

        <button type="submit" class="btn btn-primary">
            Save Task
        </button>

        <a href="{{ route('tasks.index') }}"
           class="btn btn-secondary">
            Cancel
        </a>

    </form>

</div>

@endsection

resources/views/tasks/edit.blade.php

@extends('layouts.app')

@section('content')

<div class="page-header">
    <h2>Edit Task</h2>

    <a href="{{ route('tasks.index') }}"
       class="btn btn-secondary">
        Back
    </a>
</div>

<div class="card">

    <form action="{{ route('tasks.update', $task) }}"
          method="POST">

        @csrf
        @method('PUT')

        <div class="form-group">

            <label for="task_name">
                Task Name
            </label>

            <input type="text"
                   id="task_name"
                   name="task_name"
                   value="{{ old('task_name', $task->task_name) }}">

            @error('task_name')
                <div class="error">{{ $message }}</div>
            @enderror

        </div>

        <div class="form-group">

            <label for="description">
                Description
            </label>

            <textarea id="description"
                      name="description">{{ old('description', $task->description) }}</textarea>

            @error('description')
                <div class="error">{{ $message }}</div>
            @enderror

        </div>

        <div class="form-group">

            <label for="status">
                Status
            </label>

            <select name="status" id="status">

                <option value="Pending"
                    {{ old('status', $task->status) == 'Pending' ? 'selected' : '' }}>
                    Pending
                </option>

                <option value="Completed"
                    {{ old('status', $task->status) == 'Completed' ? 'selected' : '' }}>
                    Completed
                </option>

            </select>

            @error('status')
                <div class="error">{{ $message }}</div>
            @enderror

        </div>

        <div class="form-group">

            <label for="due_date">
                Due Date
            </label>

            <input type="date"
                   id="due_date"
                   name="due_date"
                   value="{{ old('due_date', $task->due_date ? $task->due_date->format('Y-m-d') : '') }}">

            @error('due_date')
                <div class="error">{{ $message }}</div>
            @enderror

        </div>

        <button type="submit"
                class="btn btn-primary">
            Update Task
        </button>

        <a href="{{ route('tasks.index') }}"
           class="btn btn-secondary">
            Cancel
        </a>

    </form>

</div>

@endsection

resources/views/tasks/show.blade.php

@extends('layouts.app')

@section('content')

<div class="page-header">
    <h2>Task Details</h2>

    <a href="{{ route('tasks.index') }}"
       class="btn btn-secondary">
        Back to Tasks
    </a>
</div>

<div class="card">

    <div class="task-details">

        <p>
            <strong>Task ID:</strong>
            {{ $task->id }}
        </p>

        <p>
            <strong>Task Name:</strong>
            {{ $task->task_name }}
        </p>

        <p>
            <strong>Description:</strong>
            {{ $task->description ?: 'No description' }}
        </p>

        <p>
            <strong>Status:</strong>

            @if($task->status == 'Completed')

                <span class="status completed">
                    Completed
                </span>

            @else

                <span class="status pending">
                    Pending
                </span>

            @endif
        </p>

        <p>
            <strong>Due Date:</strong>

            {{ $task->due_date
                ? $task->due_date->format('F d, Y')
                : 'No due date' }}
        </p>

        <p>
            <strong>Created:</strong>
            {{ $task->created_at->format('F d, Y h:i A') }}
        </p>

        <p>
            <strong>Last Updated:</strong>
            {{ $task->updated_at->format('F d, Y h:i A') }}
        </p>

    </div>

    <br>

    <a href="{{ route('tasks.edit', $task) }}"
       class="btn btn-warning">
        Edit Task
    </a>

    <form action="{{ route('tasks.destroy', $task) }}"
          method="POST"
          style="display:inline;"
          onsubmit="return confirm('Are you sure you want to delete this task?');">

        @csrf
        @method('DELETE')

        <button type="submit"
                class="btn btn-danger">
            Delete Task
        </button>

    </form>

</div>

@endsection

php artisan serve

INFO  Server running on [http://127.0.0.1:8000].

http://127.0.0.1:8000

+ Add Task

Task Name: Finish Laravel Project
Description: Complete the CRUD task manager.
Status: Pending
Due Date: 2026-09-30

ID | Task Name | Description | Status | Due Date | Actions

Edit

Update Task

Pending

Completed

Delete

personal-task-manager/
│
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       └── TaskController.php
│   │
│   └── Models/
│       └── Task.php
│
├── database/
│   └── migrations/
│       └── xxxx_xx_xx_create_tasks_table.php
│
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── app.blade.php
│       │
│       └── tasks/
│           ├── index.blade.php
│           ├── create.blade.php
│           ├── edit.blade.php
│           └── show.blade.php
│
├── routes/
│   └── web.php
│
└── .env

