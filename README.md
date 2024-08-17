<div>Teachable Machine Image Model - p5.js and ml5.js</div>
<script src="https://cdn.jsdelivr.net/npm/p5@latest/lib/p5.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/p5@latest/lib/addons/p5.dom.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/ml5@latest/dist/ml5.min.js"></script>
<script type="text/javascript">
  // ตัวแปรสำหรับ Classifier
  let classifier;
  // URL ของโมเดล
  let imageModelURL = 'https://teachablemachine.withgoogle.com/models/knjkUfL47/';
  
  // ตัวแปรวิดีโอ
  let video;
  // ตัวแปรสำหรับเก็บผลลัพธ์การจัดหมวดหมู่
  let label = "";

  // โหลดโมเดลก่อน
  function preload() {
    classifier = ml5.imageClassifier(imageModelURL + 'model.json');
  }

  function setup() {
    createCanvas(320, 260);
    // สร้างวิดีโอ
    video = createCapture(VIDEO);
    video.size(320, 240);
    video.hide();

    // เริ่มการจัดหมวดหมู่วิดีโอ
    classifyVideo();
  }

  function draw() {
    background(0);
    // วาดวิดีโอ (พลิกในแนวนอน)
    push();
    translate(video.width, 0);
    scale(-1, 1);
    image(video, 0, 0);
    pop();

    // วาดป้ายข้อความ
    fill(255);
    textSize(16);
    textAlign(CENTER);
    text(label, width / 2, height - 4);
  }

  // ทำการคาดการณ์วิดีโอเฟรมปัจจุบัน
  function classifyVideo() {
    classifier.classify(video, gotResult);
  }

  // เมื่อเราได้ผลลัพธ์
  function gotResult(error, results) {
    if (error) {
      console.error(error);
      return;
    }
    // เก็บผลลัพธ์จากการจัดหมวดหมู่
    label = results[0].label;
    // ทำการจัดหมวดหมู่อีกครั้ง
    classifyVideo();
  }
</script>
