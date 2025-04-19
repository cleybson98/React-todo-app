import { useEffect, useState } from "react";

export default function App() {
  const [todos, setTodos] = useState(() => {
    const stored = localStorage.getItem("todos");
    return stored ? JSON.parse(stored) : [];
  });

  const [newTodo, setNewTodo] = useState("");
  const [filter, setFilter] = useState("all");

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  const handleAdd = () => {
    if (!newTodo.trim()) return;
    const todo = {
      id: Date.now(),
      text: newTodo,
      completed: false,
    };
    setTodos([todo, ...todos]);
    setNewTodo("");
  };

  const handleDelete = (id) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  const toggleComplete = (id) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  const filteredTodos = todos.filter((todo) => {
    if (filter === "completed") return todo.completed;
    if (filter === "pending") return !todo.completed;
    return true;
  });

  return (
    <div className="min-h-screen bg-gray-100 p-4 flex flex-col items-center">
      <h1 className="text-2xl font-bold mb-4">To-Do List</h1>
      <div className="flex gap-2 mb-4">
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="Digite uma tarefa"
          className="border rounded px-2 py-1"
        />
        <button
          onClick={handleAdd}
          className="bg-blue-500 text-white px-3 py-1 rounded"
        >
          Adicionar
        </button>
      </div>

      <div className="mb-4 flex gap-2">
        <button onClick={() => setFilter("all")} className="px-2 py-1 border rounded">
          Todas
        </button>
        <button onClick={() => setFilter("completed")} className="px-2 py-1 border rounded">
          Concluídas
        </button>
        <button onClick={() => setFilter("pending")} className="px-2 py-1 border rounded">
          Pendentes
        </button>
      </div>

      <ul className="w-full max-w-md">
        {filteredTodos.map((todo) => (
          <li
            key={todo.id}
            className="flex justify-between items-center bg-white p-2 mb-2 rounded shadow"
          >
            <span
              onClick={() => toggleComplete(todo.id)}
              className={`cursor-pointer ${todo.completed ? "line-through text-gray-500" : ""}`}
            >
              {todo.text}
            </span>
            <button
              onClick={() => handleDelete(todo.id)}
              className="text-red-500 hover:underline"
            >
              Remover
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
