<template>
    <div>
      <div class="d-flex align-items-center w-100" style="height: 100vh;">
          <div class="container">
              <div class="row justify-content-center">
                  <div class="col-lg-6 text-center">
                      <h2 class="py-3 fadeInText">Welcome to <span class="fs-1 fw-bold">Genetic Algorithms</span></h2>
                      <p class="fadeInText2" style="line-height: 2rem;">Genetic Algorithm (GA) เป็นเทคนิคคำนวณที่จำลองกระบวนการพัฒนาพันธุกรรมในธรรมชาติ เพื่อหาคำตอบที่เหมาะสมสำหรับปัญหา optimization ที่ซับซ้อน โดยนำหลักการจำลองการสืบพันธุ์และการควบคุมความหลากหลายมาใช้ เป็นเครื่องมือที่มีประสิทธิภาพในการแก้ไขปัญหาที่มีพื้นที่ค้นหาใหญ่และที่มีลักษณะที่ไม่แน่นอน การใช้ GA มีทั้งในด้านวิทยาศาสตร์ และงานอุตสาหกรรม เช่นในการจำลองการทำงานของระบบที่ซับซ้อน การออกแบบโครงสร้างของเครื่องจักร หรือการเรียงลำดับงานในโปรเซสผลิต การทำงานของ GA สามารถสรุปได้ในขั้นตอนพื้นฐานคือการสร้างสปาห์เชิงสุ่ม, การคัดเลือก, การคัดเลือกอย่างดี, การสร้างลูกหลาน, และการปรับปรุงพันธุกรรม.</p>
                      <a type="button" class="btn rounded rounded-pill btn-outline-primary fw-bold mt-4 px-3" href="set">Get start</a>
                  </div>
              </div>
          </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'HomePage',
    layout: 'default'
  }
  </script>
  
  <style>
          /* CSS animation to gradually reveal the text */
          @keyframes fadeInText {
              0% {
                  opacity: 0;
              }
  
              100% {
                  opacity: 1;
              }
          }
  
          /* Apply the animation to the text */
          .fadeInText {
              opacity: 0;
              animation: fadeInText 0.9s ease forwards;
          }
          .fadeInText2 {
              opacity: 0;
              animation: fadeInText 1.4s ease forwards;
          }
  </style>
  