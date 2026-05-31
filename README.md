<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>โปรเจกต์แรกของฉันบน GitHub</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-r from-blue-100 to-purple-100 min-h-screen flex items-center justify-center">

  <div class="bg-white p-8 rounded-2xl shadow-xl text-center max-w-sm w-full mx-4">
    <h1 class="text-3xl font-bold text-purple-600 mb-2">ยินดีด้วย! 🎉</h1>
    <p class="text-gray-600 mb-6">นี่คือเว็บไซต์แรกของคุณที่รันบน GitHub Pages</p>
    
    <button onclick="sayHello()" class="bg-purple-600 hover:bg-purple-700 text-white font-medium py-2.5 px-6 rounded-full transition duration-300 transform hover:scale-105">
      คลิกตรงนี้ซิ!
    </button>
    
    <p id="resultText" class="mt-4 text-lg font-semibold text-green-600 min-h-[28px]"></p>
  </div>

  <script>
    // ฟังก์ชัน JavaScript ง่ายๆ เมื่อมีการกดปุ่ม
    function sayHello() {
      const messages = [
        "ยินดีต้อนรับสู่โลกของ Developer! 💻",
        "โค้ดนี้ทำงานได้สมบูรณ์แบบ! ✨",
        "คุณเก่งมากที่เริ่มต้นสร้างสิ่งนี้ได้! 🚀",
        "ขอให้สนุกกับการเขียนโค้ดนะ! ❤️"
      ];
      
      // สุ่มข้อความจากอาเรย์มาแสดง
      const randomIndex = Math.floor(Math.random() * messages.length);
      document.getElementById('resultText').innerText = messages[randomIndex];
    }
  </script>
</body>
</html>
