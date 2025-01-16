<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GPA Calculator</title>
  <style>
    body {
      font-family: fx-CW Series, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #1C4E80;
    }
    .container {
      max-width: 600px;
      margin: 20px auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    h1 {
      text-align: center;
      color: #333;
    }
    label {
      font-weight: bold;
      margin-top: 10px;
    }
    input, select {
      width: 70%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    .button-group {
      display: flex;
      justify-content: space-between;
      margin-top: 10px;
    }
    button {
      padding: 10px;
      flex: 1;
      margin: 0 5px;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      opacity: 0.9;
    }
    button:first-child {
      background-color: #28a745; /* Green for Add Course */
    }
    button:nth-child(2) {
      background-color: #20283E; /* Blue for Calculate GPA */
    }
    button:last-child {
      background-color: #dc3545; /* Red for Reset */
    }
    .results, .course-list {
      margin-top: 20px;
      padding: 10px;
      background: #f9f9f9;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .results p, .course-list p {
      margin: 5px 0;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>GPA Calculator</h1>
    <form id="gpaForm">
      <label for="courseCode">Course Code:</label>
      <input type="text" id="courseCode" placeholder="Enter course code (e.g., BMKT2323)" required>

      <label for="credits">Credit Hours:</label>
      <input type="number" id="credits" placeholder="Enter credit hours" required>

      <label for="grade">Grade:</label>
      <select id="grade" required>
        <option value="" disabled selected>Select grade</option>
        <option value="4.0">A  (4.0)</option>
        <option value="3.7">A- (3.7)</option>
        <option value="3.3">B+ (3.3)</option>
        <option value="3.0">B  (3.0)</option>
        <option value="2.7">B- (2.7)</option>
        <option value="2.3">C+ (2.3)</option>
        <option value="2.0">C  (2.0)</option>
        <option value="1.7">C- (1.7)</option>
        <option value="1.0">D  (1.0)</option>
        <option value="0.0">F  (0.0)</option>
      </select>

      <div class="button-group">
        <button type="button" onclick="addCourse()">Add Course</button>
        <button type="button" onclick="calculateGPA()">Calculate GPA</button>
        <button type="button" onclick="resetForm()">Reset</button>
      </div>
    </form>

    <div id="courseList" class="course-list">
      <h2>Added Courses:</h2>
      <p>No courses added yet.</p>
    </div>
    
    <div id="results" class="results"></div>
  </div>

  <script>
    const courses = [];

    function addCourse() {
      const courseCode = document.getElementById("courseCode").value.trim();
      const credits = parseFloat(document.getElementById("credits").value);
      const grade = parseFloat(document.getElementById("grade").value);

      if (!courseCode || isNaN(credits) || credits <= 0 || isNaN(grade)) {
        alert("Please enter valid course details.");
        return;
      }

      courses.push({ courseCode, credits, grade });
      updateCourseList();
      alert(`Course "${courseCode}" added successfully!`);
      document.getElementById("gpaForm").reset();
    }

    function updateCourseList() {
      const courseListDiv = document.getElementById("courseList");
      if (courses.length === 0) {
        courseListDiv.innerHTML = `<h2>Added Courses:</h2><p>No courses added yet.</p>`;
        return;
      }

      courseListDiv.innerHTML = `<h2>Added Courses:</h2>`;
      courses.forEach(({ courseCode, credits, grade }, index) => {
        courseListDiv.innerHTML += `<p>${index + 1}. <strong>${courseCode}</strong>: Credits = ${credits}, Grade = ${grade}</p>`;
      });
    }

    function calculateGPA() {
      if (courses.length === 0) {
        alert("No courses added yet!");
        return;
      }

      let totalWeightedPoints = 0;
      let totalCredits = 0;

      courses.forEach(({ credits, grade }) => {
        totalWeightedPoints += credits * grade;
        totalCredits += credits;
      });

      const gpa = totalWeightedPoints / totalCredits;
      displayResults(gpa);
    }

    function displayResults(gpa) {
      const resultsDiv = document.getElementById("results");
      resultsDiv.innerHTML = "<h2>Results:</h2>";
      resultsDiv.innerHTML += `<p><strong>GPA:</strong> ${gpa.toFixed(2)}</p>`;
    }

    function resetForm() {
      document.getElementById("gpaForm").reset();
      courses.length = 0;
      updateCourseList();
      const resultsDiv = document.getElementById("results");
      resultsDiv.innerHTML = "";
    }
  </script>
</body>
</html>
