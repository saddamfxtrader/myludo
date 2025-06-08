<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Ludo Admin Panel</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body { font-family: Arial; padding: 20px; background: #f1f1f1; }
    .container { max-width: 400px; margin: auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    input, select, button { width: 100%; padding: 10px; margin: 10px 0; }
    h2 { text-align: center; }
  </style>
</head>
<body>
  <div class="container">
    <h2>Ludo Admin Panel</h2>
    <label>Transaction ID</label>
    <input type="text" id="txnId" placeholder="Enter transaction ID">

    <label>Status</label>
    <select id="status">
      <option value="approved">Approved</option>
      <option value="pending">Pending</option>
      <option value="rejected">Rejected</option>
    </select>

    <label>Group ID</label>
    <select id="groupId">
      <option value="groupA">Group A (2 players)</option>
      <option value="groupB">Group B (3 players)</option>
      <option value="groupC">Group C (4 players)</option>
    </select>

    <button onclick="updateTransaction()">✅ Approve & Assign Group</button>

    <p id="msg" style="color: green; text-align: center;"></p>
  </div>

  <!-- Firebase Scripts -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

  <script>
    // TODO: Change to your Firebase Config
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://YOUR_PROJECT.firebaseio.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT.appspot.com",
      messagingSenderId: "XXXXXX",
      appId: "1:XXXX:web:XXXX"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function updateTransaction() {
      const txnId = document.getElementById("txnId").value.trim();
      const status = document.getElementById("status").value;
      const groupId = document.getElementById("groupId").value;

      if (!txnId) {
        alert("Please enter a Transaction ID");
        return;
      }

      // Search by txnId
      db.ref("transactions").orderByChild("txnId").equalTo(txnId).once("value", snapshot => {
        if (snapshot.exists()) {
          snapshot.forEach(child => {
            db.ref("transactions/" + child.key).update({
              status: status,
              groupId: groupId
            });
          });
          document.getElementById("msg").innerText = "✅ Updated successfully!";
        } else {
          alert("Transaction ID not found!");
        }
      });
    }
  </script>
</body>
</html>
