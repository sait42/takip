# takip
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kolay Takip</title>
  <style>
    body { background-color: #F5F5F5; font-family: Arial, sans-serif; color: #333; margin: 0; padding: 0; }
    .container { max-width: 960px; margin: 0 auto; padding: 20px; }
    .auth-section { min-height: 100vh; display: flex; align-items: center; justify-content: center; }
    .card { background-color: white; border-radius: 12px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); padding: 24px; }
    .input { width: 100%; padding: 8px; margin-bottom: 16px; border: 1px solid #ccc; border-radius: 8px; box-sizing: border-box; }
    .input:focus { outline: none; border-color: #40E0D0; box-shadow: 0 0 0 2px rgba(64, 224, 208, 0.2); }
    .button { padding: 8px 16px; border-radius: 8px; color: white; background-color: #40E0D0; border: none; cursor: pointer; margin-right: 8px; transition: background-color 0.3s; }
    .button:hover { background-color: #36C7B8; }
    .button-red { background-color: #EF4444; }
    .button-red:hover { background-color: #DC2626; }
    .flex { display: flex; gap: 8px; flex-wrap: wrap; }
    .hidden { display: none; }
    .table { border-collapse: collapse; width: 100%; }
    .table th, .table td { border: 1px solid #999; padding: 8px; text-align: center; }
    .table thead tr:first-child { background-color: #1E3A8A; color: white; font-weight: bold; }
    .table thead tr:nth-child(2) { background-color: #BFDBFE; color: black; font-weight: bold; }
    .table tbody tr:nth-child(even) { background-color: #F3F4F6; }
    .table tbody tr:nth-child(odd) { background-color: white; }
    h1 { color: #40E0D0; text-align: center; font-size: 2.5rem; margin-bottom: 24px; }
    h2 { color: #40E0D0; font-size: 1.5rem; margin-bottom: 16px; }
    h3 { color: #40E0D0; font-size: 1.25rem; margin-bottom: 16px; }
    .text-center { text-align: center; }
    .text-gray { color: #6B7280; }
    .font-bold { font-weight: bold; }
    .space-y-6 > * + * { margin-top: 1.5rem; }
    .space-x-4 > * + * { margin-left: 1rem; }
    .mt-4 { margin-top: 1rem; }
    .mt-6 { margin-top: 1.5rem; }
    .mb-4 { margin-bottom: 1rem; }
    .mb-6 { margin-bottom: 1.5rem; }
    .mb-2 { margin-bottom: 0.5rem; }
    .py-4 { padding-top: 1rem; padding-bottom: 1rem; }
    .p-3 { padding: 0.75rem; }
    .border { border: 1px solid #D1D5DB; }
  </style>
</head>
<body>
  <div id="app" class="container">
    <!-- Login/Register Section -->
    <div id="auth-section" class="auth-section">
      <div class="card" style="width: 100%; max-width: 400px;">
        <h1>Kolay Takip</h1>
        <div class="space-y-6">
          <input id="username" type="text" placeholder="Kullanıcı Adı" class="input">
          <input id="password" type="password" placeholder="Şifre" class="input">
          <div class="flex">
            <button id="login-button" class="button" style="width: 100%;">Giriş Yap</button>
            <button id="register-button" class="button" style="width: 100%;">Kayıt Ol</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Main App Section -->
    <div id="main-section" class="hidden">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px;">
        <h1>Kolay Takip</h1>
        <button id="logout-button" class="button">Çıkış Yap</button>
      </div>

      <!-- Teacher View -->
      <div id="teacher-view" class="hidden">
        <div class="card mb-6">
          <h2>Deneme Yönetimi</h2>
          <div class="flex mb-4">
            <input id="exam-name" type="text" placeholder="Deneme Adı" class="input">
            <button id="add-exam-button" class="button">Deneme Ekle</button>
          </div>
          <h3>Mevcut Denemeler</h3>
          <select id="exam-delete-select" class="input mb-4">
            <option value="">Deneme Seç</option>
          </select>
          <button id="delete-exam-button" class="button button-red">Deneme Sil</button>
        </div>
        <div class="card mb-6">
          <h2>Analiz</h2>
          <div class="flex mb-6">
            <button id="analyze-by-exam-button" class="button">Deneme Bazlı Analiz</button>
            <button id="analyze-by-student-button" class="button">Öğrenci Bazlı Analiz</button>
            <button id="recalculate-lgs-button" class="button">Tüm LGS Puanlarını Yeniden Hesapla</button>
          </div>
          <div id="analysis-result" class="mt-4"></div>
        </div>
        <div class="card mb-6">
          <h2>Öğrenci Performans Raporu</h2>
          <button id="generate-report-button" class="button">Rapor Oluştur</button>
          <div id="performance-report" class="mt-4"></div>
        </div>
        <div class="card">
          <h2>Tüm Veriler</h2>
          <div class="flex mb-4">
            <select id="student-filter" class="input" onchange="renderTeacherData()">
              <option value="">Tüm Öğrenciler</option>
            </select>
            <select id="exam-filter" class="input" onchange="renderTeacherData()">
              <option value="">Tüm Denemeler</option>
            </select>
            <button id="sort-by-student-button" class="button">Öğrenci Bazlı Sırala</button>
            <button id="sort-by-exam-button" class="button">Deneme Bazlı Sırala</button>
          </div>
          <div id="teacher-data-table"></div>
        </div>
      </div>

      <!-- Student View -->
      <div id="student-view" class="hidden">
        <div class="card mb-6">
          <h2>Veri Girişi</h2>
          <select id="exam-select" class="input mb-4">
            <option value="">Deneme Seç</option>
          </select>
          <table class="table mb-4">
            <thead>
              <tr>
                <th class="p-3">Ders</th>
                <th colspan="3" class="p-3">Doğru - Yanlış - Boş</th>
              </tr>
            </thead>
            <tbody id="student-input-table"></tbody>
          </table>
          <button id="save-student-data-button" class="button">Kaydet</button>
        </div>
        <div class="card">
          <h2>Verilerim</h2>
          <div id="student-data-table"></div>
          <button id="export-data-button" class="button mt-6">Verileri Dışa Aktar</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    const subjects = ["Türkçe", "İnkılap", "Din Kültürü", "İngilizce", "Matematik", "Fen Bilimleri"];
    const coefficients = {
      "Türkçe": 3.6714,
      "İnkılap": 1.6849,  // Sosyal
      "Din Kültürü": 1.632,  // Din
      "İngilizce": 1.9407,
      "Matematik": 4.9527,
      "Fen Bilimleri": 4.0725
    };
    const baseScore = 193.492;
    let currentUser = null;
    let users = JSON.parse(localStorage.getItem("users")) || {};
    let exams = JSON.parse(localStorage.getItem("exams")) || [];
    let examData = JSON.parse(localStorage.getItem("examData")) || {};

    // Olay Dinleyicileri
    document.getElementById("login-button").addEventListener("click", login);
    document.getElementById("register-button").addEventListener("click", register);
    document.getElementById("logout-button").addEventListener("click", logout);
    document.getElementById("add-exam-button").addEventListener("click", addExam);
    document.getElementById("delete-exam-button").addEventListener("click", deleteExam);
    document.getElementById("analyze-by-exam-button").addEventListener("click", analyzeByExam);
    document.getElementById("analyze-by-student-button").addEventListener("click", analyzeByStudent);
    document.getElementById("sort-by-student-button").addEventListener("click", sortByStudent);
    document.getElementById("sort-by-exam-button").addEventListener("click", sortByExam);
    document.getElementById("save-student-data-button").addEventListener("click", saveStudentData);
    document.getElementById("export-data-button").addEventListener("click", exportData);
    document.getElementById("generate-report-button").addEventListener("click", generatePerformanceReport);
    document.getElementById("recalculate-lgs-button").addEventListener("click", recalculateLGSForAll);

    function saveToLocalStorage() {
      localStorage.setItem("users", JSON.stringify(users));
      localStorage.setItem("exams", JSON.stringify(exams));
      localStorage.setItem("examData", JSON.stringify(examData));
    }

    function login() {
      console.log("login() fonksiyonu çağrıldı!");
      const username = document.getElementById("username").value.trim().toLowerCase();
      const password = document.getElementById("password").value;
      if (users[username] && users[username].password === password) {
        currentUser = { username, role: users[username].role };
        document.getElementById("auth-section").classList.add("hidden");
        document.getElementById("main-section").classList.remove("hidden");
        if (currentUser.role === "teacher") {
          document.getElementById("teacher-view").classList.remove("hidden");
          populateExamSelect();
          populateDeleteExamSelect();
          populateStudentFilter();
          populateExamFilter();
          renderTeacherData();
        } else {
          document.getElementById("student-view").classList.remove("hidden");
          populateExamSelect();
          renderStudentInputTable();
          renderStudentData();
        }
      } else {
        alert("Geçersiz kullanıcı adı veya şifre!");
      }
      document.getElementById("username").value = "";
      document.getElementById("password").value = "";
    }

    function register() {
      console.log("register() fonksiyonu çağrıldı!");
      const username = document.getElementById("username").value.trim().toLowerCase();
      const password = document.getElementById("password").value;
      if (!username || !password) {
        alert("Lütfen kullanıcı adı ve şifreyi doldurun!");
        return;
      }
      if (users[username]) {
        alert("Bu kullanıcı adı zaten alınmış!");
        return;
      }
      const role = username === "ogretmen" ? "teacher" : "student";
      users[username] = { password, role };
      saveToLocalStorage();
      alert("Kayıt başarılı! Giriş yapabilirsiniz.");
      document.getElementById("username").value = "";
      document.getElementById("password").value = "";
    }

    function logout() {
      currentUser = null;
      document.getElementById("main-section").classList.add("hidden");
      document.getElementById("teacher-view").classList.add("hidden");
      document.getElementById("student-view").classList.add("hidden");
      document.getElementById("auth-section").classList.remove("hidden");
    }

    function addExam() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen deneme ekleyebilir!");
        return;
      }
      const examName = document.getElementById("exam-name").value.trim();
      if (!examName) {
        alert("Lütfen bir deneme adı girin!");
        return;
      }
      if (!exams.includes(examName)) {
        exams.push(examName);
        saveToLocalStorage();
        populateExamSelect();
        populateDeleteExamSelect();
        populateExamFilter();
        renderTeacherData();
        alert("Deneme eklendi!");
      } else {
        alert("Bu deneme adı zaten mevcut!");
      }
      document.getElementById("exam-name").value = "";
    }

    function deleteExam() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen deneme silebilir!");
        return;
      }
      const examName = document.getElementById("exam-delete-select").value;
      if (!examName) {
        alert("Lütfen bir deneme seçin!");
        return;
      }
      if (confirm(`"${examName}" denemesini ve ilgili tüm verileri silmek istediğinize emin misiniz?`)) {
        exams = exams.filter(exam => exam !== examName);
        for (let username in examData) {
          examData[username] = examData[username].filter(entry => entry.exam !== examName);
          if (examData[username].length === 0) {
            delete examData[username];
          }
        }
        saveToLocalStorage();
        populateExamSelect();
        populateDeleteExamSelect();
        populateExamFilter();
        populateStudentFilter();
        renderTeacherData();
        alert(`"${examName}" denemesi ve ilgili veriler silindi!`);
      }
    }

    function populateExamSelect() {
      const examSelect = document.getElementById("exam-select");
      examSelect.innerHTML = '<option value="">Deneme Seç</option>';
      exams.forEach(exam => {
        examSelect.innerHTML += `<option value="${exam}">${exam}</option>`;
      });
    }

    function populateDeleteExamSelect() {
      const examDeleteSelect = document.getElementById("exam-delete-select");
      examDeleteSelect.innerHTML = '<option value="">Deneme Seç</option>';
      exams.forEach(exam => {
        examDeleteSelect.innerHTML += `<option value="${exam}">${exam}</option>`;
      });
    }

    function populateStudentFilter() {
      const studentFilter = document.getElementById("student-filter");
      studentFilter.innerHTML = '<option value="">Tüm Öğrenciler</option>';
      for (let username in examData) {
        if (examData[username].length > 0) {
          studentFilter.innerHTML += `<option value="${username}">${username}</option>`;
        }
      }
    }

    function populateExamFilter() {
      const examFilter = document.getElementById("exam-filter");
      examFilter.innerHTML = '<option value="">Tüm Denemeler</option>';
      const uniqueExams = new Set();
      for (let username in examData) {
        examData[username].forEach(entry => uniqueExams.add(entry.exam));
      }
      uniqueExams.forEach(exam => {
        examFilter.innerHTML += `<option value="${exam}">${exam}</option>`;
      });
    }

    function renderStudentInputTable() {
      const tableBody = document.getElementById("student-input-table");
      tableBody.innerHTML = "";
      subjects.forEach(subject => {
        tableBody.innerHTML += `
          <tr>
            <td class="p-3 border">${subject}</td>
            <td class="p-3 border"><input type="number" id="${subject}-dogru" class="input" min="0"></td>
            <td class="p-3 border"><input type="number" id="${subject}-yanlis" class="input" min="0"></td>
            <td class="p-3 border"><input type="number" id="${subject}-bos" class="input" min="0"></td>
          </tr>
        `;
      });
    }

    function calculateNet(correct, wrong) {
      const net = correct - (wrong / 3);
      return net < 0 ? 0 : net;
    }

    function calculateLGS(subjectData) {
      let totalScore = baseScore;
      subjects.forEach(subject => {
        const net = subjectData[subject].net;
        totalScore += net * coefficients[subject];
      });
      return totalScore.toFixed(2);
    }

    function saveStudentData() {
      if (currentUser.role !== "student") {
        alert("Sadece öğrenciler veri girebilir!");
        return;
      }
      const examName = document.getElementById("exam-select").value;
      if (!examName) {
        alert("Lütfen bir deneme seçin!");
        return;
      }

      if (examData[currentUser.username]) {
        const existingExam = examData[currentUser.username].find(entry => entry.exam === examName);
        if (existingExam) {
          alert("Bu denemeyi silmeden tekrar veri giremezsiniz!");
          return;
        }
      }

      const data = { exam: examName };
      let totalCorrect = 0, totalWrong = 0, totalEmpty = 0;
      subjects.forEach(subject => {
        const correct = parseInt(document.getElementById(`${subject}-dogru`).value) || 0;
        const wrong = parseInt(document.getElementById(`${subject}-yanlis`).value) || 0;
        const empty = parseInt(document.getElementById(`${subject}-bos`).value) || 0;
        const net = calculateNet(correct, wrong);
        data[subject] = { correct, wrong, empty, net };
        totalCorrect += correct;
        totalWrong += wrong; // Düzeltildi: net yerine wrong kullanıldı
        totalEmpty += empty;
      });
      data.total = {
        correct: totalCorrect,
        wrong: totalWrong,
        empty: totalEmpty
      };
      data.total.lgs = calculateLGS(data);

      if (!examData[currentUser.username]) {
        examData[currentUser.username] = [];
      }
      examData[currentUser.username].push(data);
      saveToLocalStorage();
      renderStudentData();
      alert("Veriler kaydedildi!");
      subjects.forEach(subject => {
        document.getElementById(`${subject}-dogru`).value = "";
        document.getElementById(`${subject}-yanlis`).value = "";
        document.getElementById(`${subject}-bos`).value = "";
      });
    }

    function renderStudentData() {
      const table = document.getElementById("student-data-table");
      table.innerHTML = "";
      if (!examData[currentUser.username] || examData[currentUser.username].length === 0) {
        table.innerHTML = "<p class='text-center text-gray py-4'>Henüz veri yok.</p>";
        return;
      }

      let html = `
        <table class="table">
          <thead>
            <tr>
              <th class="p-3">SIRA</th>
              <th class="p-3">Deneme Adı</th>
              ${subjects.map(subject => `<th class="p-3">${subject}</th>`).join("")}
              <th class="p-3">TOPLAM</th>
              <th class="p-3">LGS PUANI</th>
              <th class="p-3">İşlem</th>
            </tr>
            <tr>
              <th class="p-3"></th>
              <th class="p-3"></th>
              ${subjects.map(() => `<th class="p-3">D - Y - B - </th>`).join("")}
              <th class="p-3">D - Y - B</th>
              <th class="p-3"></th>
              <th class="p-3"></th>
            </tr>
          </thead>
          <tbody>
      `;
      examData[currentUser.username].forEach((entry, index) => {
        html += `
          <tr>
            <td class="p-3 border">${index + 1}</td>
            <td class="p-3 border">${entry.exam}</td>
            ${subjects.map(subject => `
              <td class="p-3 border">
                ${entry[subject].correct} - ${entry[subject].wrong} - ${entry[subject].empty}
              </td>
            `).join("")}
            <td class="p-3 border">
              ${entry.total.correct} - ${entry.total.wrong} - ${entry.total.empty}
            </td>
            <td class="p-3 border">${entry.total.lgs}</td>
            <td class="p-3 border">
              <button onclick="deleteStudentData(${index})" class="button button-red">Sil</button>
            </td>
          </tr>
        `;
      });
      html += "</tbody></table>";
      table.innerHTML = html;
    }

    function deleteStudentData(index) {
      if (currentUser.role !== "student") {
        alert("Sadece öğrenciler kendi verilerini silebilir!");
        return;
      }
      if (confirm("Bu veriyi silmek istediğinize emin misiniz?")) {
        examData[currentUser.username].splice(index, 1);
        saveToLocalStorage();
        renderStudentData();
      }
    }

    function renderTeacherData() {
      if (currentUser.role !== "teacher") return;
      const table = document.getElementById("teacher-data-table");
      const studentFilter = document.getElementById("student-filter").value;
      const examFilter = document.getElementById("exam-filter").value;
      table.innerHTML = "";
      let html = `
        <table class="table">
          <thead>
            <tr>
              <th class="p-3">Öğrenci</th>
              <th class="p-3">Deneme Adı</th>
              ${subjects.map(subject => `<th class="p-3">${subject}</th>`).join("")}
              <th class="p-3">Toplam</th>
              <th class="p-3">LGS Puanı</th>
            </tr>
            <tr>
              <th class="p-3"></th>
              <th class="p-3"></th>
              ${subjects.map(() => `<th class="p-3">D - Y - B</th>`).join("")}
              <th class="p-3">D - Y - B</th>
              <th class="p-3"></th>
            </tr>
          </thead>
          <tbody>
      `;
      let hasData = false;
      for (const username in examData) {
        if (studentFilter && username !== studentFilter) continue;
        examData[username].forEach(entry => {
          if (examFilter && entry.exam !== examFilter) return;
          hasData = true;
          html += `
            <tr>
              <td class="p-3 border">${username}</td>
              <td class="p-3 border">${entry.exam}</td>
              ${subjects.map(subject => `
                <td class="p-3 border">
                  ${entry[subject].correct} - ${entry[subject].wrong} - ${entry[subject].empty}
                </td>
              `).join("")}
              <td class="p-3 border">
                ${entry.total.correct} - ${entry.total.wrong} - ${entry.total.empty}
              </td>
              <td class="p-3 border">${entry.total.lgs}</td>
            </tr>
          `;
        });
      }
      if (!hasData) {
        html += `<tr><td colspan="${subjects.length + 4}" class="p-3 border text-center text-gray">Henüz veri yok.</td></tr>`;
      }
      html += "</tbody></table>";
      table.innerHTML = html;
    }

    function analyzeByExam() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen analiz yapabilir!");
        return;
      }
      const resultDiv = document.getElementById("analysis-result");
      let analysis = {};
      for (const username in examData) {
        examData[username].forEach(entry => {
          if (!analysis[entry.exam]) {
            analysis[entry.exam] = { totalLGS: 0, count: 0 };
          }
          analysis[entry.exam].totalLGS += parseFloat(entry.total.lgs);
          analysis[entry.exam].count += 1;
        });
      }
      let html = "<h3>Deneme Bazlı Analiz</h3>";
      if (Object.keys(analysis).length === 0) {
        html += "<p class='text-gray'>Henüz analiz edilecek veri yok.</p>";
      } else {
        for (const exam in analysis) {
          const avgLGS = (analysis[exam].totalLGS / analysis[exam].count).toFixed(2);
          html += `<p class='mb-2'>${exam}: Ortalama LGS Puanı: <span class='font-bold'>${avgLGS}</span> (${analysis[exam].count} öğrenci)</p>`;
        }
      }
      resultDiv.innerHTML = html;
    }

    function analyzeByStudent() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen analiz yapabilir!");
        return;
      }
      const resultDiv = document.getElementById("analysis-result");
      let analysis = {};
      for (const username in examData) {
        analysis[username] = { totalLGS: 0, count: 0 };
        examData[username].forEach(entry => {
          analysis[username].totalLGS += parseFloat(entry.total.lgs);
          analysis[username].count += 1;
        });
      }
      let html = "<h3>Öğrenci Bazlı Analiz</h3>";
      if (Object.keys(analysis).length === 0) {
        html += "<p class='text-gray'>Henüz analiz edilecek veri yok.</p>";
      } else {
        for (const username in analysis) {
          const avgLGS = (analysis[username].totalLGS / analysis[username].count).toFixed(2);
          html += `<p class='mb-2'>${username}: Ortalama LGS Puanı: <span class='font-bold'>${avgLGS}</span> (${analysis[username].count} deneme)</p>`;
        }
      }
      resultDiv.innerHTML = html;
    }

    function exportData() {
      if (currentUser.role !== "student") {
        alert("Sadece öğrenciler verilerini dışa aktarabilir!");
        return;
      }
      const data = JSON.stringify(examData[currentUser.username] || []);
      const blob = new Blob([data], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "kolay-takip-veriler.json";
      a.click();
      URL.revokeObjectURL(url);
    }

    function sortByStudent() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen bu işlemi yapabilir!");
        return;
      }
      const studentFilter = document.getElementById("student-filter").value;
      if (!studentFilter) {
        alert("Lütfen bir öğrenci seçin!");
        return;
      }
      if (examData[studentFilter]) {
        examData[studentFilter].sort((a, b) => a.exam.localeCompare(b.exam));
        saveToLocalStorage();
        renderTeacherData();
        alert(`${studentFilter} için denemeler sıralandı!`);
      } else {
        alert("Seçilen öğrenci için veri yok!");
      }
    }

    function sortByExam() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen bu işlemi yapabilir!");
        return;
      }
      const examFilter = document.getElementById("exam-filter").value;
      if (!examFilter) {
        alert("Lütfen bir deneme seçin!");
        return;
      }
      let allData = [];
      for (let username in examData) {
        examData[username].forEach(entry => {
          if (entry.exam === examFilter) {
            allData.push({ username, entry });
          }
        });
      }
      if (allData.length > 0) {
        allData.sort((a, b) => a.username.localeCompare(b.username));
        renderSortedExamData(allData);
        alert(`${examFilter} için öğrenciler sıralandı!`);
      } else {
        alert("Seçilen deneme için veri yok!");
      }
    }

    function renderSortedExamData(sortedData) {
      const table = document.getElementById("teacher-data-table");
      table.innerHTML = "";
      let html = `
        <table class="table">
          <thead>
            <tr>
              <th class="p-3">Öğrenci</th>
              <th class="p-3">Deneme Adı</th>
              ${subjects.map(subject => `<th class="p-3">${subject}</th>`).join("")}
              <th class="p-3">Toplam</th>
              <th class="p-3">LGS Puanı</th>
            </tr>
            <tr>
              <th class="p-3"></th>
              <th class="p-3"></th>
              ${subjects.map(() => `<th class="p-3">D - Y - B</th>`).join("")}
              <th class="p-3">D - Y - B</th>
              <th class="p-3"></th>
            </tr>
          </thead>
          <tbody>
      `;
      sortedData.forEach(item => {
        const entry = item.entry;
        html += `
          <tr>
            <td class="p-3 border">${item.username}</td>
            <td class="p-3 border">${entry.exam}</td>
            ${subjects.map(subject => `
              <td class="p-3 border">
                ${entry[subject].correct} - ${entry[subject].wrong} - ${entry[subject].empty}
              </td>
            `).join("")}
            <td class="p-3 border">
              ${entry.total.correct} - ${entry.total.wrong} - ${entry.total.empty}
            </td>
            <td class="p-3 border">${entry.total.lgs}</td>
          </tr>
        `;
      });
      html += "</tbody></table>";
      table.innerHTML = html;
    }

    function generatePerformanceReport() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen rapor oluşturabilir!");
        return;
      }
      const reportDiv = document.getElementById("performance-report");
      let performance = {};
      for (const username in examData) {
        performance[username] = { totalLGS: 0, count: 0 };
        examData[username].forEach(entry => {
          performance[username].totalLGS += parseFloat(entry.total.lgs);
          performance[username].count += 1;
        });
      }
      let html = "<h3>Öğrenci Performans Raporu</h3>";
      if (Object.keys(performance).length === 0) {
        html += "<p class='text-gray'>Henüz raporlanacak veri yok.</p>";
      } else {
        html += `
          <table class="table mt-4">
            <thead>
              <tr>
                <th class="p-3">Öğrenci</th>
                <th class="p-3">Deneme Sayısı</th>
                <th class="p-3">Ortalama LGS Puanı</th>
              </tr>
            </thead>
            <tbody>
        `;
        for (const username in performance) {
          const avgLGS = (performance[username].totalLGS / performance[username].count).toFixed(2);
          html += `
            <tr>
              <td class="p-3 border">${username}</td>
              <td class="p-3 border">${performance[username].count}</td>
              <td class="p-3 border">${avgLGS}</td>
            </tr>
          `;
        }
        html += "</tbody></table>";
      }
      reportDiv.innerHTML = html;
    }

    function recalculateLGSForAll() {
      if (currentUser.role !== "teacher") {
        alert("Sadece öğretmen LGS puanlarını yeniden hesaplayabilir!");
        return;
      }
      for (let username in examData) {
        examData[username].forEach(entry => {
          let totalCorrect = 0, totalWrong = 0, totalEmpty = 0;
          subjects.forEach(subject => {
            const correct = entry[subject].correct;
            const wrong = entry[subject].wrong;
            const empty = entry[subject].empty;
            const net = calculateNet(correct, wrong);
            entry[subject].net = net;
            totalCorrect += correct;
            totalWrong += wrong;
            totalEmpty += empty;
          });
          entry.total = {
            correct: totalCorrect,
            wrong: totalWrong,
            empty: totalEmpty
          };
          entry.total.lgs = calculateLGS(entry);
        });
      }
      saveToLocalStorage();
      renderTeacherData();
      alert("Tüm LGS puanları yeniden hesaplandı ve güncellendi!");
    }
  </script>
</body>
</html>
