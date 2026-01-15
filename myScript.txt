document.addEventListener("DOMContentLoaded", () => {
  const editBtn = document.getElementById("edit-btn");
  const table = document.getElementById("table");
  const themeBtns = document.getElementsByName("theme");
  const customThemeColor = document.getElementById("custom-theme-color");
  const body = document.querySelector("body");
  const header = document.querySelector("header");
  const footer = document.querySelector("footer");
  const buttons = document.getElementsByClassName("button");
  const tableHeadings = document.querySelectorAll("th");
  const tableRows = document.querySelectorAll("tr");

  let isEditing = false;

  // Edit button logic
  editBtn.addEventListener("click", () => {
    isEditing = !isEditing;
    editBtn.textContent = isEditing ? "Save" : "Edit";

    const rows = table.querySelectorAll("tbody tr");

    rows.forEach((row) => {
      const cells = row.querySelectorAll("td");

      cells.forEach((cell, i) => {
        if (i === 0) return; // skip "Days" column

        if (isEditing) {
          // Switch to select
          const cellData = cell.textContent.trim();
          const wasFree = cell.classList.contains("free-period");

          cell.innerHTML = `
            <select name="subject" class="subject-select">
              <option value="${cellData}" selected>${cellData}</option>
              <option value="English">English</option>
              <option value="Chemistry">Chemistry</option>
              <option value="Math">Math</option>
              <option value="Urdu">Urdu</option>
              <option value="Computer Science">Computer Science</option>
              <option value="Biology">Biology</option>
              <option value="Free Period">Free Period</option>
              <option value="Test">Test</option>
              <option value="Exam">Exam</option>
            </select>
          `;
        } else {
          // Save selected value
          const subjectSelect = cell.querySelector(".subject-select");

          if (subjectSelect) {
            const subject = subjectSelect.value;
            cell.textContent = subject;

            // Highlight free, test, exam
            cell.classList.remove("free-period", "test", "exam");
            if (subject.toLowerCase().includes("free")) {
              cell.classList.add("free-period");
            } else if (subject.toLowerCase().includes("test")) {
              cell.classList.add("test");
            } else if (subject.toLowerCase().includes("exam")) {
              cell.classList.add("exam");
            }
          }
        }
      });
    });
  });

  // Theme change logic
  let currentTheme = document.querySelector(
    'input[name="theme"]:checked'
  ).value;
  body.classList.add(currentTheme);

  themeBtns.forEach((btn) => {
    btn.addEventListener("click", () => {
      body.classList.remove(currentTheme);
      body.classList.add(btn.value);
      currentTheme = btn.value;
      removeBgColor();
    });
  });

  customThemeColor.addEventListener("change", () => {
    header.style.backgroundColor = customThemeColor.value;
    footer.style.backgroundColor = customThemeColor.value;
    buttons[0].style.backgroundColor = customThemeColor.value;
    buttons[1].style.backgroundColor = customThemeColor.value;

    tableHeadings.forEach((th) => {
      th.style.backgroundColor = customThemeColor.value;
    });
    tableRows.forEach((tr, index) => {
      if (index % 2 === 0) {
        tr.style.backgroundColor = customThemeColor.value + "56";
      }
    });
  });

  function removeBgColor() {
    header.style.removeProperty("background-color");
    footer.style.removeProperty("background-color");
    buttons[0].style.removeProperty("background-color");
    buttons[1].style.removeProperty("background-color");

    tableHeadings.forEach((th) => {
      th.style.removeProperty("background-color");
    });
    tableRows.forEach((tr, index) => {
      if (index % 2 === 0) {
        tr.style.removeProperty("background-color");
      }
    });
  }
});
