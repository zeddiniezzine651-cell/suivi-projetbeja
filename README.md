<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>متابعة المشاريع - بلدية باجة</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f8f9fa; font-family: sans-serif; padding: 20px; }
        .card-main { border-radius: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); background: white; padding: 20px; }
        .beja-title { color: #004a99; border-bottom: 2px solid #004a99; padding-bottom: 10px; margin-bottom: 20px; }
        @media print { .no-print { display: none; } }
    </style>
</head>
<body>

<div class="container card-main">
    <div class="text-center">
        <h2 class="beja-title">بلدية باجة - متابعة المشاريع</h2>
    </div>

    <div class="table-responsive">
        <table class="table table-striped align-middle text-center">
            <thead class="table-dark">
                <tr>
                    <th>المشروع</th>
                    <th>الميزانية</th>
                    <th>المقاول</th>
                    <th>الحالة</th>
                </tr>
            </thead>
            <tbody id="project-list">
                <tr><td colspan="4">جاري تحميل المشاريع...</td></tr>
            </tbody>
        </table>
    </div>

    <div class="text-center mt-3 no-print">
        <button class="btn btn-primary" onclick="window.print()">طباعة التقرير</button>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

    const firebaseConfig = {
        databaseURL: "https://suivi-projets-4817b-default-rtdb.firebaseio.com"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    const projectsRef = ref(db, 'projects');

 
