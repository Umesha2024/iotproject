<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>IoT Monitoring Dashboard</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #eef2f3;
      padding: 20px;
      text-align: center;
    }
    h1 {
      color: #333;
    }
    .sensor-box {
      display: inline-block;
      background: #fff;
      border: 1px solid #ccc;
      margin: 10px;
      padding: 20px;
      border-radius: 10px;
      width: 200px;
      box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
    }
    .value {
      font-size: 2em;
      color: #0077cc;
    }
    .label {
      font-size: 1.1em;
      margin-top: 10px;
      color: #666;
    }
  </style>
</head>
<body>

  <h1>🏠 SMART HOME</h1>

  <div class="sensor-box">
    <div class="value" id="flame">--</div>
    <div class="label">Flame Detected</div>
  </div>

  <div class="sensor-box">
    <div class="value" id="distance">--</div>
    <div class="label">Person Detection (cm)</div>
  </div>

  <div class="sensor-box">
    <div class="value" id="rain">--</div>
    <div class="label">Rain Status</div>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

  <script>
    // ✅ Replace with your Firebase config
    const firebaseConfig = {
      apiKey: "AIzaSyDP7OFx8sC1J3C6rfXi9zZBuOZLCmWWGqY",
      authDomain: "myprojectbasedoniot.firebaseapp.com",
      databaseURL: "https://myprojectbasedoniot-default-rtdb.firebaseio.com",
      projectId: "myprojectbasedoniot",
      storageBucket: "myprojectbasedoniot.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function updateElement(id, value, suffix = "") {
      document.getElementById(id).textContent = value + suffix;
    }

    // Flame Sensor
    db.ref("flameSensor/value").on("value", snapshot => {
      const val = snapshot.val();
      updateElement("flame", val === 1 ? "🔥 Yes" : "❌ No");
    });

    // Person Detection via Distance Sensor
    db.ref("distanceSensor/value").on("value", snapshot => {
      const distance = snapshot.val();
      updateElement("distance", distance + " cm");
    });

    // Rain Sensor
    db.ref("rainSensor/value").on("value", snapshot => {
      const val = snapshot.val();
      updateElement("rain", val === 1 ? "🌧 Yes" : "☀ No");
    });
  </script>

</body>
</html>
