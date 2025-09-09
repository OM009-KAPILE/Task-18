import React, { useState } from "react";

const App = () => {
  const [tasks, setTasks] = useState({
    todo: ["Learn React", "Build Kanban Board"],
    inprogress: ["Study Drag & Drop"],
    done: ["Setup Project"],
  });

  const onDragStart = (e, task, fromColumn) => {
    e.dataTransfer.setData("task", task);
    e.dataTransfer.setData("fromColumn", fromColumn);
  };

  const onDrop = (e, toColumn) => {
    const task = e.dataTransfer.getData("task");
    const fromColumn = e.dataTransfer.getData("fromColumn");

    if (fromColumn === toColumn) return;

    const updatedTasks = { ...tasks };
    updatedTasks[fromColumn] = updatedTasks[fromColumn].filter((t) => t !== task);
    updatedTasks[toColumn].push(task);

    setTasks(updatedTasks);
  };

  const allowDrop = (e) => {
    e.preventDefault();
  };

  return (
    <div className="flex gap-6 p-8 bg-gray-100 min-h-screen">
      {Object.keys(tasks).map((column) => (
        <div
          key={column}
          className="flex-1 bg-white shadow-lg rounded-xl p-4"
          onDrop={(e) => onDrop(e, column)}
          onDragOver={allowDrop}
        >
          <h2 className="text-lg font-bold mb-4 capitalize">
            {column.replace("inprogress", "In Progress")}
          </h2>
          {tasks[column].map((task, index) => (
            <div
              key={index}
              draggable
              onDragStart={(e) => onDragStart(e, task, column)}
              className="p-3 mb-2 bg-blue-100 rounded-lg shadow cursor-move"
            >
              {task}
            </div>
          ))}
        </div>
      ))}
    </div>
  );
};

export default App;
