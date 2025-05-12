# Survey Form Builder (ReactJS)
## Date:12-05-2025

## AIM
To create a Survey Form Builder using ReactJS..

## ALGORITHM
### STEP 1 Initialize States
mode ← 'build' (default mode)

questions ← [] (holds all survey questions)

currentQuestion ← { text: '', type: 'text', options: '' } (holds input while building)

editingIndex ← null (tracks which question is being edited)

responses ← {} (user responses to survey)

submitted ← false (indicates whether survey was submitted)

### STEP 2 Switch Modes
Toggle mode between 'build' and 'fill' when the toggle button is clicked.

Reset submitted to false when entering Fill Mode.

### STEP 3 Build Mode Logic
#### a. Add/Update Question
If currentQuestion.text is not empty:

If type is "radio" or "checkbox", split currentQuestion.options by commas → optionsArray.

Construct a questionObject:

If editingIndex is not null, replace question at that index.

Else, push questionObject into questions.

Reset currentQuestion and editingIndex.

#### b. Edit Question
Set currentQuestion to the selected question’s data.

Set editingIndex to the question’s index.

#### c. Delete Question
Remove the selected question from questions.

### STEP 4 Fill Mode Logic
#### a. Render Form
For each question in questions:

If type is "text", render a text input.

If type is "radio", render radio buttons for each option.

If type is "checkbox", render checkboxes.

#### b. Capture Responses
On input change:

For "text" and "radio", update responses[index] = value.

For "checkbox":

If option exists in responses[index], remove it.

Else, add it to the array.

### STEP 5 Submit Form
When user clicks "Submit":

Set submitted = true.

Display a summary using the questions and responses.

### STEP 6 Display Summary
Iterate over questions.

Display the response from responses[i] for each question:

If it's an array, join it with commas.

If empty, show "No response".


## PROGRAM
App.JS
```
import React, { useState } from 'react';
import './App.css'; // Optional styling

function App() {
  const [mode, setMode] = useState('build'); // 'build' or 'fill'
  const [questions, setQuestions] = useState([]);
  const [newQuestion, setNewQuestion] = useState({ text: '', type: 'text', options: '' });
  const [responses, setResponses] = useState({});
  const [submitted, setSubmitted] = useState(false);

  // Handle adding a question
  const addQuestion = () => {
    if (!newQuestion.text.trim()) return;
    const id = Date.now();
    const options = ['radio', 'checkbox'].includes(newQuestion.type)
      ? newQuestion.options.split(',').map(opt => opt.trim())
      : [];
    setQuestions([...questions, { id, ...newQuestion, options }]);
    setNewQuestion({ text: '', type: 'text', options: '' });
  };

  // Handle deleting a question
  const removeQuestion = (id) => {
    setQuestions(questions.filter(q => q.id !== id));
  };

  // Handle change in Fill Mode
  const handleResponseChange = (id, value) => {
    setResponses({ ...responses, [id]: value });
  };

  // Checkbox handler
  const handleCheckboxChange = (id, option) => {
    const existing = responses[id] || [];
    const updated = existing.includes(option)
      ? existing.filter(item => item !== option)
      : [...existing, option];
    setResponses({ ...responses, [id]: updated });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setSubmitted(true);
  };

  return (
    <div className="App" style={{ padding: '20px' }}>
      <button onClick={() => {
        setMode(mode === 'build' ? 'fill' : 'build');
        setSubmitted(false);
      }}>
        Switch to {mode === 'build' ? 'Fill Mode' : 'Build Mode'}
      </button>

      <h1>{mode === 'build' ? 'Survey Builder' : 'Survey Filler'}</h1>

      {mode === 'build' ? (
        <div>
          <input
            type="text"
            placeholder="Question text"
            value={newQuestion.text}
            onChange={(e) => setNewQuestion({ ...newQuestion, text: e.target.value })}
          />
          <select
            value={newQuestion.type}
            onChange={(e) => setNewQuestion({ ...newQuestion, type: e.target.value })}
          >
            <option value="text">Text</option>
            <option value="radio">Multiple Choice (Radio)</option>
            <option value="checkbox">Multiple Select (Checkbox)</option>
          </select>
          {['radio', 'checkbox'].includes(newQuestion.type) && (
            <input
              type="text"
              placeholder="Options (comma-separated)"
              value={newQuestion.options}
              onChange={(e) => setNewQuestion({ ...newQuestion, options: e.target.value })}
            />
          )}
          <button onClick={addQuestion}>Add Question</button>

          <ul>
            {questions.map((q) => (
              <li key={q.id}>
                {q.text} ({q.type})
                <button onClick={() => removeQuestion(q.id)}>Remove</button>
              </li>
            ))}
          </ul>
        </div>
      ) : (
        <form onSubmit={handleSubmit}>
          {questions.map((q) => (
            <div key={q.id} style={{ marginBottom: '20px' }}>
              <label><strong>{q.text}</strong></label>
              <div>
                {q.type === 'text' && (
                  <input
                    type="text"
                    onChange={(e) => handleResponseChange(q.id, e.target.value)}
                  />
                )}
                {q.type === 'radio' && q.options.map(opt => (
                  <label key={opt}>
                    <input
                      type="radio"
                      name={q.id}
                      value={opt}
                      onChange={(e) => handleResponseChange(q.id, e.target.value)}
                    />
                    {opt}
                  </label>
                ))}
                {q.type === 'checkbox' && q.options.map(opt => (
                  <label key={opt}>
                    <input
                      type="checkbox"
                      value={opt}
                      onChange={() => handleCheckboxChange(q.id, opt)}
                    />
                    {opt}
                  </label>
                ))}
              </div>
            </div>
          ))}
          <button type="submit">Submit</button>
        </form>
      )}

      {submitted && (
        <div>
          <h2>Response Summary</h2>
          <ul>
            {questions.map((q) => (
              <li key={q.id}>
                <strong>{q.text}:</strong> {Array.isArray(responses[q.id]) ? responses[q.id].join(', ') : responses[q.id] || 'No response'}
              </li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}

export default App;
```

## OUTPUT
![image](https://github.com/user-attachments/assets/52c63918-be50-46ec-8e48-59b68d6f0d03)

![image](https://github.com/user-attachments/assets/7620a9af-6dcb-4266-afcd-acf1975f6aac)

## RESULT
The program for creating Survey Form Builder using ReactJS is executed successfully.
