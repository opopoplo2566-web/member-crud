<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ระบบบันทึกข้อมูลสมาชิก (CRUD)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  
  <style>
    /* ลบขอบส้มเวลาโฟกัสอินพุต */
    input:focus {
      outline: none;
    }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-8">

  <div class="max-w-4xl mx-auto bg-white rounded-lg shadow-md p-6">
    <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">ระบบบันทึกข้อมูลสมาชิก (CRUD)</h1>

    <form id="memberForm" class="mb-8 bg-gray-50 p-4 rounded-md border border-gray-200">
      <input type="hidden" id="memberId">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div>
          <label class="block text-sm font-medium text-gray-700">ชื่อ-นามสกุล</label>
          <input type="text" id="memberName" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700">อีเมล</label>
          <input type="email" id="memberEmail" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700">เบอร์โทรศัพท์</label>
          <input type="tel" id="memberPhone" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:ring-blue-500 focus:border-blue-500">
        </div>
      </div>
      <div class="mt-4 flex justify-end gap-2">
        <button type="button" id="cancelBtn" class="hidden px-4 py-2 bg-gray-400 text-white rounded-md hover:bg-gray-500">ยกเลิก</button>
        <button type="submit" id="submitBtn" class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700">บันทึกข้อมูล</button>
      </div>
    </form>

    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">ชื่อ-นามสกุล</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">อีเมล</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">เบอร์โทร</th>
            <th class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase">จัดการ</th>
          </tr>
        </thead>
        <tbody id="memberTableBody" class="bg-white divide-y divide-gray-200">
          </tbody>
      </table>
    </div>
  </div>

  <script>
    // ดึงตัวแปรจาก DOM
    const memberForm = document.getElementById('memberForm');
    const memberIdInput = document.getElementById('memberId');
    const nameInput = document.getElementById('memberName');
    const emailInput = document.getElementById('memberEmail');
    const phoneInput = document.getElementById('memberPhone');
    const submitBtn = document.getElementById('submitBtn');
    const cancelBtn = document.getElementById('cancelBtn');
    const tableBody = document.getElementById('memberTableBody');

    // ดึงข้อมูลจาก LocalStorage (ถ้าไม่มีให้เป็นอาร์เรย์ว่าง)
    let members = JSON.parse(localStorage.getItem('members')) || [];

    // ฟังก์ชันสำหรับแสดงข้อมูลในตาราง (Read)
    function renderTable() {
      tableBody.innerHTML = '';
      
      if (members.length === 0) {
        tableBody.innerHTML = `
          <tr>
            <td colspan="4" class="px-6 py-4 text-center text-gray-500">ยังไม่มีข้อมูลสมาชิก</td>
          </tr>
        `;
        return;
      }

      members.forEach((member) => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">${member.name}</td>
          <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${member.email}</td>
          <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${member.phone}</td>
          <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
            <button onclick="editMember('${member.id}')" class="text-indigo-600 hover:text-indigo-900 mr-3">แก้ไข</button>
            <button onclick="deleteMember('${member.id}')" class="text-red-600 hover:text-red-900">ลบ</button>
          </td>
        `;
        tableBody.appendChild(tr);
      });
    }

    // ฟังก์ชันบันทึกข้อมูล (Create & Update)
    memberForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const id = memberIdInput.value;
      const name = nameInput.value;
      const email = emailInput.value;
      const phone = phoneInput.value;

      if (id) {
        // โหมดแก้ไขข้อมูล (Update)
        members = members.map(m => m.id === id ? { id, name, email, phone } : m);
      } else {
        // โหมดเพิ่มข้อมูลใหม่ (Create)
        const newMember = {
          id: Date.now().toString(), // ใช้ timestamp เป็น ID แบบง่าย
          name,
          email,
          phone
        };
        members.push(newMember);
      }

      saveAndRefresh();
      resetForm();
    });

    // ฟังก์ชันเตรียมข้อมูลใส่ฟอร์มเพื่อแก้ไข (Edit Mode)
    window.editMember = function(id) {
      const member = members.find(m => m.id === id);
      if (member) {
        memberIdInput.value = member.id;
        nameInput.value = member.name;
        emailInput.value = member.email;
        phoneInput.value = member.phone;

        submitBtn.textContent = 'อัปเดตข้อมูล';
        cancelBtn.classList.remove('hidden');
      }
    }

    // ฟังก์ชันลบข้อมูล (Delete)
    window.deleteMember = function(id) {
      if (confirm('คุณแน่ใจใช่ไหมที่จะลบสมาชิกคนนี้?')) {
        members = members.filter(m => m.id !== id);
        saveAndRefresh();
        resetForm();
      }
    }

    // ฟังก์ชันล้างฟอร์ม
    function resetForm() {
      memberForm.reset();
      memberIdInput.value = '';
      submitBtn.textContent = 'บันทึกข้อมูล';
      cancelBtn.classList.add('hidden');
    }

    // ฟังก์ชันเซฟลง LocalStorage และอัปเดตหน้าจอ
    function saveAndRefresh() {
      localStorage.setItem('members', JSON.stringify(members));
      renderTable();
    }

    // กดปุ่มยกเลิกเวลาแก้ไข
    cancelBtn.addEventListener('click', resetForm);

    // เรียกใช้งานตอนโหลดหน้าเว็บครั้งแรก
    renderTable();
  </script>
</body>
</html>
