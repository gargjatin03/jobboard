import React, { useState, useEffect } from "react";
import axios from "axios";

export default function JobBoard() {
  const [jobs, setJobs] = useState([]);
  const [newJob, setNewJob] = useState({ title: "", company: "", description: "" });

  useEffect(() => {
    axios.get("http://localhost:5000/jobs").then((res) => setJobs(res.data));
  }, []);

  const handleSubmit = (e) => {
    e.preventDefault();
    axios.post("http://localhost:5000/jobs", newJob).then((res) => {
      setJobs([...jobs, res.data]);
      setNewJob({ title: "", company: "", description: "" });
    });
  };

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold">Job Board</h1>
      <form onSubmit={handleSubmit} className="space-y-4 mt-4">
        <input
          type="text"
          placeholder="Job Title"
          className="border p-2 w-full"
          value={newJob.title}
          onChange={(e) => setNewJob({ ...newJob, title: e.target.value })}
        />
        <input
          type="text"
          placeholder="Company Name"
          className="border p-2 w-full"
          value={newJob.company}
          onChange={(e) => setNewJob({ ...newJob, company: e.target.value })}
        />
        <textarea
          placeholder="Job Description"
          className="border p-2 w-full"
          value={newJob.description}
          onChange={(e) => setNewJob({ ...newJob, description: e.target.value })}
        />
        <button type="submit" className="bg-blue-500 text-white p-2 rounded-lg">
          Post Job
        </button>
      </form>
      <div className="mt-6">
        <h2 className="text-xl font-semibold">Available Jobs</h2>
        <ul>
          {jobs.map((job, index) => (
            <li key={index} className="border p-4 mt-2">
              <h3 className="text-lg font-bold">{job.title}</h3>
              <p className="text-gray-600">{job.company}</p>
              <p>{job.description}</p>
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

