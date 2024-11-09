HTML
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Date and Real-Time Clock</title>
<link rel="stylesheet" href="style.css"> <!-- Link to the external CSS file -->
</head>
<body>
  <div class="clock-container">
    <div class="date" id="currentDate"></div>
    <div class="time" id="currentTime"></div>
  </div>

<script>
// Set a specific date
const specificDate = "09 November 2024";

function updateTime() {
  const date = new Date();

  // Get the current time
  const hours = String(date.getHours()).padStart(2, '0');
  const minutes = String(date.getMinutes()).padStart(2, '0');
  const seconds = String(date.getSeconds()).padStart(2, '0');

  // Display the specific date and current time
  document.getElementById('currentDate').innerText = specificDate;
  document.getElementById('currentTime').innerText = `${hours}:${minutes}:${seconds}`;
}

// Update the time every second
setInterval(updateTime, 1000);

// Initial call
updateTime();
</script>
</body>
</html>
//////////////////////

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Display CSV Data</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <h2>CSV Data from File</h2>

  <!-- Display message if no data is found -->
  <div id="message" style="color: red;"></div>

  <!-- Table to display CSV data -->
  <table id="csvTable" border="1" style="width: 100%; border-collapse: collapse; margin-top: 10px;">
    <thead></thead>
    <tbody></tbody>
  </table>

  <script>
    // Fetch the CSV file from the project folder
    fetch('data.csv')
      .then(response => {
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        return response.text();
      })
      .then(data => {
        // Check if data is empty
        if (!data.trim()) {
          displayMessage('CSV file is empty or has no data.');
          return;
        }
        
        // Process and display CSV data
        const rows = data.split('\n').filter(row => row.trim()); // Remove empty rows
        if (rows.length === 0) {
          displayMessage('No valid data found in CSV file.');
          return;
        }

        displayTable(rows);
      })
      .catch(error => {
        console.error('Error loading CSV file:', error);
        displayMessage('Failed to load the CSV file.');
      });

    function displayTable(rows) {
      const table = document.getElementById('csvTable');
      const thead = table.querySelector('thead');
      const tbody = table.querySelector('tbody');
      
      // Clear any previous table content
      thead.innerHTML = '';
      tbody.innerHTML = '';
      
      // Process header row (first row)
      const headerRow = document.createElement('tr');
      const headers = rows[0].split(',').map(cell => cell.trim());
      headers.forEach(cell => {
        const th = document.createElement('th');
        th.textContent = cell;
        headerRow.appendChild(th);
      });
      thead.appendChild(headerRow);

      // Process data rows (remaining rows)
      rows.slice(1).forEach(row => {
        const columns = row.split(',').map(cell => cell.trim());
        const tr = document.createElement('tr');
        columns.forEach(cell => {
          const td = document.createElement('td');
          td.textContent = cell;
          tr.appendChild(td);
        });
        tbody.appendChild(tr);
      });
    }

    // Function to display error or informational messages
    function displayMessage(msg) {
      const messageDiv = document.getElementById('message');
      messageDiv.textContent = msg;
    }
  </script>

</body>
</html>
