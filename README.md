<!DOCTYPE html>
<html lang="ms">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Semakan ID DELIMa Murid</title>
  <style>
    :root {
      --primary: #0d6efd;
      --bg: #f8f9fa;
      --card-bg: #ffffff;
      --text: #212529;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      background: var(--card-bg);
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 480px;
    }

    h2 {
      text-align: center;
      color: #0b5ed7;
      margin-bottom: 8px;
    }

    p.subtitle {
      text-align: center;
      color: #6c757d;
      font-size: 0.9em;
      margin-bottom: 25px;
    }

    .form-group {
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
    }

    input[type="text"] {
      width: 100%;
      padding: 12px;
      border: 1px solid #ced4da;
      border-radius: 6px;
      box-sizing: border-box;
      font-size: 1em;
    }

    button {
      width: 100%;
      padding: 12px;
      background-color: var(--primary);
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 1em;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background-color: #0b5ed7;
    }

    .result-card {
      margin-top: 25px;
      padding: 15px;
      border-radius: 8px;
      background: #e7f1ff;
      border: 1px solid #b6d4fe;
      display: none;
    }

    .result-item {
      margin-bottom: 10px;
    }

    .result-item strong {
      display: inline-block;
      width: 120px;
    }

    .error {
      margin-top: 20px;
      padding: 12px;
      background: #f8d7da;
      color: #842029;
      border-radius: 6px;
      display: none;
      text-align: center;
    }

    .loader {
      display: none;
      text-align: center;
      margin-top: 15px;
      font-weight: bold;
      color: var(--primary);
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>Semakan ID DELIMa</h2>
    <p class="subtitle">Sila masukkan No. MyKid / MyKad murid untuk semakan</p>

    <div class="form-group">
      <label for="mykid">No. MyKid / MyKad (tanpa -):</label>
      <input type="text" id="mykid" placeholder="Contoh: 120304011234">
    </div>

    <button onclick="semakID()">Cari Maklumat</button>

    <div class="loader" id="loader">Sedang mencari data...</div>
    <div class="error" id="error">Data murid tidak dijumpai. Sila semak nombor MyKid.</div>

    <div class="result-card" id="resultCard">
      <div class="result-item"><strong>Nama:</strong> <span id="resNama"></span></div>
      <div class="result-item"><strong>Kelas:</strong> <span id="resKelas"></span></div>
      <div class="result-item"><strong>ID DELIMa:</strong> <span id="resID"></span></div>
      <div class="result-item"><strong>Kata Laluan:</strong> <span id="resPass"></span></div>
    </div>
  </div>

  <script>
    // TAMPAL URL WEB APP GOOGLE APPS SCRIPT ANDA DI SINI
    const SCRIPT_URL = "ISI_URL_APPS_SCRIPT_ANDA_DI_SINI";

    function semakID() {
      const inputVal = document.getElementById('mykid').value.trim();
      const loader = document.getElementById('loader');
      const errorDiv = document.getElementById('error');
      const resultCard = document.getElementById('resultCard');

      if (!inputVal) {
        alert("Sila masukkan No. MyKid/MyKad murid.");
        return;
      }

      loader.style.display = 'block';
      errorDiv.style.display = 'none';
      resultCard.style.display = 'none';

      fetch(`${SCRIPT_URL}?mykid=${encodeURIComponent(inputVal)}`)
        .then(response => response.json())
        .then(data => {
          loader.style.display = 'none';
          if (data.error) {
            errorDiv.innerText = data.error;
            errorDiv.style.display = 'block';
          } else {
            document.getElementById('resNama').innerText = data.nama;
            document.getElementById('resKelas').innerText = data.kelas;
            document.getElementById('resID').innerText = data.id;
            document.getElementById('resPass').innerText = data.pass;
            resultCard.style.display = 'block';
          }
        })
        .catch(err => {
          loader.style.display = 'none';
          errorDiv.innerText = "Ralat sistem. Pastikan URL Google Apps Script diisi dengan betul.";
          errorDiv.style.display = 'block';
        });
    }
  </script>
</body>
</html>
