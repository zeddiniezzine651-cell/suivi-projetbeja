
 
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بلدية باجة - متابعة المشاريع</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f4f7f6; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; padding: 15px; }
        .main-card { border-radius: 15px; box-shadow: 0 8px 20px rgba(0,0,0,0.12); background: white; padding: 20px; border-top: 5px solid #004a99; margin-top: 20px; }
        .beja-header { color: #004a99; margin-bottom: 25px; font-weight: bold; }
        .table-custom thead { background-color: #004a99; color: white; }
        .badge-status { font-size: 0.9rem; padding: 8px 12px; }
        @media print { .no-print { display: none; } }
    </style>
</head>
<body>

<div class="container main-card">
    <div class="text-center">
        <h2 class="beja-header">بلدية باجة <br> <small class="text-muted fs-5">منظومة متابعة المشاريع العمومية</small></h2>
    </div>

    <div class="table-responsive">
        <table class="table table-hover align-middle text-center shadow-sm">
            <thead class="table-custom">
                <tr>
                    <th>المشروع</th>
                    <th>الميزانية</th>
                    <th>المقاول</th>
                    <th>الوضعية</th>
                </tr>
            </thead>
            <tbody id="display-table">
                <tr><td colspan="4" class="py-5 text-muted">جاري الاتصال بالسحابة...</td></tr>
            </tbody>
        </table>
    </div>

    <div class="text-center mt-4 no-print">
        <button class="btn btn-dark btn-lg px-5" onclick="window.print()">🖨️ طباعة التقرير</button>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

    // الإعدادات من واقع الصورة التي أرفقتها
    const firebaseConfig = {
        databaseURL: "https://suivi-projets-4817b-default-rtdb.firebaseio.com"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    const projectsRef = ref(db, 'projects');
    const tableBody = document.getElementById('display-table');

    // الاستماع للبيانات وعرضها
    onValue(projectsRef, (snapshot) => {
        const data = snapshot.val();
        tableBody.innerHTML = ""; 

        if (data) {
            // تحويل الكائن إلى مصفوفة ومعالجته
            Object.keys(data).forEach(key => {
                const project = data[key];
                
                // تحديد لون الحالة بناءً على النص
                let statusClass = "bg-secondary";
                if (project.status === "مكتمل") statusClass = "bg-success";
                if (project.status === "قيد الإنجاز") statusClass = "bg-warning text-dark";
                if (project.status === "متوقف") statusClass = "bg-danger";

                const row = `
                    <tr>
                        <td class="fw-bold">${project.name || 'غير معرف'}</td>
                        <td class="text-primary fw-bold">${project.budget || '0'} د.ت</td>
                        <td>${project.contractor || '---'}</td>
                        <td><span class="badge rounded-pill ${statusClass} badge-status">${project.status || 'غير محدد'}</span></td>
                    </tr>
                `;
                tableBody.innerHTML += row;
            });
        } else {
            tableBody.innerHTML = '<tr><td colspan="4" class="py-5 text-muted">لا توجد مشاريع مسجلة في قاعدة البيانات حالياً.</td></tr>';
        }
    }, (error) => {
        tableBody.innerHTML = `<tr><td colspan="4" class="py-5 text-danger">خطأ في الاتصال: ${error.message}</td></tr>`;
    });
</script>

</body>
</html
