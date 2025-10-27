<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>案件查詢系統</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", sans-serif;
      background: #f8f9fa;
      text-align: center;
      padding: 30px;
    }
    h2 { color: #333; }
    input {
      padding: 10px;
      width: 80%;
      max-width: 300px;
      border: 1px solid #aaa;
      border-radius: 5px;
    }
    button {
      padding: 10px 20px;
      margin-left: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
    }
    button:hover { background-color: #0056b3; }
    #result {
      margin-top: 30px;
      font-size: 1.1em;
      text-align: left;
      max-width: 400px;
      margin-left: auto;
      margin-right: auto;
      background: #fff;
      border-radius: 8px;
      padding: 15px;
      box-shadow: 0 0 8px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h2>案件資料查詢</h2>
  <input type="text" id="nameInput" placeholder="請輸入名字">
  <button onclick="searchData()">查詢</button>
  <div id="result"></div>

  <script>
  const apiURL = "https://script.google.com/macros/s/AKfycbyV5s7vvYKSaWG7SCZU4Hoz2cpFmdalc9b84l15FEm1NrTV54AK6XvkwDZYtV79ESJx/exec";

  async function searchData() {
  const name = document.getElementById("nameInput").value.trim();
  const resultDiv = document.getElementById("result");

  if (!name) {
    resultDiv.innerHTML = "<p>⚠️ 請輸入名字。</p>";
    return;
  }

  resultDiv.innerHTML = "<p>🔄 查詢中，請稍候...</p>"; // 顯示 loading

  try {
    const response = await fetch(`${apiURL}?name=${encodeURIComponent(name)}`);
    const data = await response.json();

    if (Array.isArray(data)) {
      if (data.length === 0) {
        resultDiv.innerHTML = "<p>❌ 查無此資料。</p>";
      } else {
        resultDiv.innerHTML = data.map(item => `
          <div style="margin-bottom:20px; border-bottom:1px solid #ccc; padding-bottom:10px;">
            <p><strong>接件日：</strong>${item["接件日"]}</p>
            <p><strong>名字：</strong>${item["名字"]}</p>
            <p><strong>執案人1：</strong>${item["執案人1"]}</p>
            <p><strong>執案人2：</strong>${item["執案人2"]}</p>
            <p><strong>塔位：</strong>${item["塔位"]}</p>
            <p><strong>單位：</strong>${item["單位"]}</p>
            <p><strong>備註：</strong>${item["備註"]}</p>
          </div>
        `).join('');
      }
    } else if (data.error) {
      resultDiv.innerHTML = `<p>❌ ${data.error}</p>`;
    }
  } catch (err) {
    resultDiv.innerHTML = "<p>⚠️ 連線錯誤，請稍後再試。</p>";
    console.error(err);
  }
}
document.getElementById("nameInput").addEventListener("keydown", function(event) {
  if (event.key === "Enter") {
    searchData();
  }
});
</script></body>
</html>
