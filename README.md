<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>JobSnap • Resume Analyzer + Templates</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(135deg,#03bde6,#10b981,#34d399);min-height:100vh;color:#1f2937;overflow-x:hidden;}
nav{position:fixed;top:0;left:0;right:0;background:rgba(255,255,255,0.95);backdrop-filter:blur(20px);z-index:1000;padding:1rem 0;box-shadow:0 4px 20px rgba(0,0,0,0.1);}
.nav-container{max-width:1200px;margin:0 auto;display:flex;justify-content:center;gap:2rem;}
.nav-btn{background:none;border:none;font-size:1.1rem;font-weight:700;color:#1f2937;cursor:pointer;padding:0.5rem 1rem;border-radius:12px;transition:all 0.3s ease;position:relative;}
.nav-btn.active{background:#d809e2;color:white;box-shadow:0 8px 25px rgba(16,185,129,0.4);}
.nav-btn:hover{transform:translateY(-2px);}
.page{display:none;padding:2rem;min-height:100vh;padding-top:120px;max-width:1200px;margin:0 auto;}
.page.active{display:block;animation:fadeIn 0.5s ease;}
@keyframes fadeIn{from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);}}
header{text-align:center;padding:4rem 2rem;color:white;}
header h1{font-size:3.5rem;font-weight:800;margin-bottom:1rem;}
header p{font-size:1.3rem;opacity:0.95;}
.about-content{max-width:900px;margin:0 auto;text-align:center;line-height:1.8;}
.about-content h2{font-size:2.5rem;margin:2rem 0 1rem;color:white;font-weight:700;}
.about-content p{font-size:1.1rem;color:#e0f2fe;margin-bottom:1.5rem;}
.templates-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(350px,1fr));gap:2rem;margin:2rem 0;}
.template-card{background:white;border-radius:24px;padding:2rem;box-shadow:0 25px 50px rgba(0,0,0,0.15);text-align:center;transition:all 0.3s ease;cursor:pointer;position:relative;overflow:hidden;}
.template-card:hover{transform:translateY(-10px);box-shadow:0 35px 70px rgba(0,0,0,0.25);}
.template-card h3{font-size:1.5rem;margin:1rem 0;color:#1f2937;font-weight:700;}
.template-card p{color:#6b7280;margin-bottom:1.5rem;}
.template-badge{position:absolute;top:20px;right:20px;background:#d8ea0e;color:white;padding:0.5rem 1rem;border-radius:50px;font-size:0.85rem;font-weight:600;}
.download-btn{background:linear-gradient(135deg,#1029b95b,#059669);color:white;border:none;padding:1rem 2.5rem;border-radius:50px;font-weight:800;font-size:1.1rem;cursor:pointer;transition:all 0.3s ease;margin:0.5rem;box-shadow:0 10px 30px rgba(16,185,129,0.4);}
.download-btn:hover{transform:translateY(-3px);box-shadow:0 15px 40px rgba(16,185,129,0.6);}
.upload-container{background:white;width:100%;max-width:500px;margin:2rem auto;padding:3rem;border-radius:24px;text-align:center;box-shadow:0 35px 70px rgba(0,0,0,0.2);}
.upload-container h2{font-size:2rem;margin-bottom:1.5rem;color:#1f2937;}
input[type="file"]{width:100%;padding:1rem;border:3px dashed #d1d5db;border-radius:20px;font-size:1rem;transition:all 0.3s ease;margin-bottom:1rem;}
input[type="file"]:hover{border-color:#c9b711;}
.btn-primary{margin-top:1.5rem;background:linear-gradient(135deg,#10b981,#059669);color:white;border:none;padding:1.2rem 3rem;border-radius:50px;font-weight:800;font-size:1.1rem;cursor:pointer;transition:all 0.3s ease;box-shadow:0 10px 30px rgba(16,185,129,0.4);}
.btn-primary:hover{transform:translateY(-3px);box-shadow:0 15px 40px rgba(16,185,129,0.6);}
#status{margin-top:1rem;font-weight:600;font-size:1.1rem;}
#filePreview{margin-top:0.5rem;font-weight:600;color:#10b981;cursor:pointer;text-decoration:underline;}
.results{background:white;width:100%;max-width:600px;margin:2rem auto;padding:3rem;border-radius:24px;box-shadow:0 35px 70px rgba(0,0,0,0.2);text-align:center;}
.score-circle{width:180px;height:180px;border-radius:50%;margin:0 auto 2rem;display:flex;align-items:center;justify-content:center;font-size:3rem;font-weight:800;background:conic-gradient(#10b981 0deg 360deg);box-shadow:0 25px 60px rgba(16,185,129,0.4);}
.score-title{font-size:1.8rem;font-weight:800;color:#374151;margin-bottom:1rem;}
.result-item{margin:1.5rem 0;padding:1.5rem;background:#f9fafb;border-radius:16px;border-left:5px solid #10b981;box-shadow:0 5px 15px rgba(0,0,0,0.08);}
.warning{border-left-color:#f59e0b;}
.section-divider{margin:3rem 0;height:1px;background:linear-gradient(90deg,transparent,#e5e7eb,transparent);}
@media(max-width:768px){.nav-container{gap:0.5rem;flex-wrap:wrap;}.nav-btn{font-size:1rem;padding:0.4rem;}.page{padding:1rem;padding-top:100px;}.header h1{font-size:2.5rem;}.templates-grid{grid-template-columns:1fr;}}
</style>
</head>
<body>
<nav>
  <div class="nav-container">
    <button class="nav-btn active" onclick="showPage('home')">🏠 Home</button>
    <button class="nav-btn" onclick="showPage('templates')">📄 Templates</button>
    <button class="nav-btn" onclick="showPage('about')">📖 About</button>
    <button class="nav-btn" onclick="showPage('upload')">⬆️ Upload</button>
    <button class="nav-btn" onclick="showPage('results')">📊 Results</button>
  </div>
</nav>

<div id="home" class="page active">
  <header>
    <h1>JobSnap</h1>
    <p>Your AI-powered resume analyzer + FREE professional templates</p>
  </header>
  <div class="about-content">
    <h2>🚀 Get Hired Faster</h2>
    <p>Download ATS-friendly templates or upload your resume for instant AI analysis.</p>
    <div style="display:flex;gap:2rem;justify-content:center;flex-wrap:wrap;margin:2rem 0;">
      <button class="btn-primary" onclick="showPage('templates')" style="font-size:1.3rem;">📄 Free Templates</button>
      <button class="btn-primary" onclick="showPage('upload')" style="font-size:1.3rem;">⬆️ Analyze Resume</button>
    </div>
  </div>
</div>

<div id="templates" class="page">
  <header>
    <h1>📄 Professional Templates</h1>
    <p>ATS-optimized, modern designs - ready to customize</p>
  </header>
  <div class="templates-grid">
    <div class="template-card" onclick="downloadTemplate('software')">
      <div class="template-badge">Software Engineer</div>
      <h3>Modern Tech Resume</h3>
      <p>Perfect for developers, perfect skills section</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
    
    <div class="template-card" onclick="downloadTemplate('marketing')">
      <div class="template-badge">Marketing</div>
      <h3>Creative Professional</h3>
      <p>Stand out with modern design + achievements</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
    
    <div class="template-card" onclick="downloadTemplate('data')">
      <div class="template-badge">Data Science</div>
      <h3>Data Scientist</h3>
      <p>Technical resume with projects showcase</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
    
    <div class="template-card" onclick="downloadTemplate('manager')">
      <div class="template-badge">Management</div>
      <h3>Executive Leader</h3>
      <p>Leadership-focused with metrics & impact</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
    
    <div class="template-card" onclick="downloadTemplate('designer')">
      <div class="template-badge">Design</div>
      <h3>Creative Designer</h3>
      <p>Portfolio-style with visual emphasis</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
    
    <div class="template-card" onclick="downloadTemplate('entry')">
      <div class="template-badge">Entry Level</div>
      <h3>Fresh Graduate</h3>
      <p>Perfect for students & career starters</p>
      <button class="download-btn">⬇️ Download PDF</button>
    </div>
  </div>
</div>

<div id="about" class="page">
  <header>
    <h1>About JobSnap</h1>
    <p>Templates + AI Analysis = Perfect Resume</p>
  </header>
  <div class="about-content">
    <h2>🎯 Everything You Need</h2>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:2rem;margin:2rem 0;">
      <div><h3 style="color:white;font-size:1.5rem;margin-bottom:1rem;">📄 6+ Templates</h3><p>ATS-optimized designs for every industry</p></div>
      <div><h3 style="color:white;font-size:1.5rem;margin-bottom:1rem;">🔍 AI Analysis</h3><p>Instant scoring + improvement suggestions</p></div>
      <div><h3 style="color:white;font-size:1.5rem;margin-bottom:1rem;">⚡ Lightning Fast</h3><p>Results in seconds, works on any device</p></div>
    </div>
    <button class="btn-primary" onclick="showPage('templates')">Get Started →</button>
  </div>
</div>

<div id="upload" class="page">
  <div class="upload-container">
    <h2>⬆️ Upload Your Resume</h2>
    <p style="color:#6b7280;margin-bottom:2rem;">Or use our <a href="#" onclick="showPage('templates');return false;" style="color:#10b981;font-weight:600;">free templates</a></p>
    <input type="file" id="resume" accept=".pdf,.doc,.docx">
    <button class="btn-primary" onclick="checkSnap()">🔍 Analyze My Resume</button>
    <p id="status"></p>
    <p id="filePreview"></p>
  </div>
</div>

<div id="results" class="page">
  <div class="results">
    <div id="resultsDiv" style="display:none;">
      <div class="score-circle" id="scoreCircle">
        <span id="scoreNumber">0%</span>
      </div>
      <div class="score-title" id="scoreTitle">JobSnap Score</div>
      
      <div class="result-item" id="keywordsResult">
        ✅ <strong>Keywords:</strong> Perfect match for target job
      </div>
      <div class="result-item" id="skillsResult">
        ✅ <strong>Skills:</strong> All essential skills detected
      </div>
      <div class="result-item" id="experienceResult">
        ✅ <strong>Experience:</strong> Strong career progression
      </div>
      <div class="result-item warning" id="formattingResult">
        ⚠️ <strong>Formatting:</strong> Minor improvements suggested
      </div>
      
      <div class="section-divider"></div>
      <button class="btn-primary" onclick="showPage('templates')" style="margin-right:1rem;">📄 New Template</button>
      <button class="btn-primary" onclick="showPage('upload')">🔄 Analyze Again</button>
    </div>
  </div>
</div>

<script>
pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

const pages = ['home', 'templates', 'about', 'upload', 'results'];
let currentPage = 'home';
let uploadedResumeURL = null;

// Sample resume template data (JSON format)
const resumeTemplates = {
  software: {
    name: "Software Engineer Template",
    content: `John Doe
Software Engineer | Full Stack Developer

john.doe@email.com | +1 (555) 123-4567 | linkedin.com/in/johndoe | github.com/johndoe

PROFESSIONAL SUMMARY
Results-driven Software Engineer with 5+ years experience building scalable web applications. Expert in React, Node.js, and cloud technologies. Delivered 20+ projects with 99.9% uptime.

TECHNICAL SKILLS
Frontend: React, JavaScript, TypeScript, HTML5, CSS3, Tailwind
Backend: Node.js, Python, Express, Django, REST APIs
Database: PostgreSQL, MongoDB, Redis
Cloud: AWS, Docker, Kubernetes
Tools: Git, CI/CD, Agile, Jira

EXPERIENCE
Senior Software Engineer | TechCorp Inc. | 2022-Present
• Developed microservices architecture reducing latency by 65%
• Led team of 8 engineers to deliver $2M project on time
• Implemented CI/CD pipeline increasing deployment speed 3x

Software Engineer | StartUpX | 2020-2022
• Built responsive React dashboard used by 10K+ users
• Optimized database queries improving performance 40%
• Integrated Stripe payments processing $1M+ monthly

EDUCATION
B.S. Computer Science | MIT | 2016-2020
GPA: 3.9/4.0 | Dean's List

PROJECTS
E-Commerce Platform | GitHub: 2K stars
Real-time chat app with WebSocket & React`
  },
  marketing: {
    name: "Marketing Professional Template",
    content: `Sarah Johnson
Digital Marketing Specialist

sarah.johnson@email.com | +1 (555) 987-6543 | linkedin.com/in/sarahj

PROFESSIONAL SUMMARY
Award-winning Digital Marketer with 7+ years experience driving 300%+ ROI campaigns. Google Ads certified with expertise in SEO, content & social media strategy.

CORE COMPETENCIES
• Google Ads | Facebook Ads | SEO | SEM
• Content Marketing | Email Automation
• Analytics | A/B Testing | Conversion Rate Optimization

PROFESSIONAL EXPERIENCE
Digital Marketing Manager | BrandGrowth | 2021-Present
• Grew organic traffic 250% through SEO strategy
• Managed $500K monthly ad spend with 4.2x ROAS
• Launched email campaigns with 45% open rates

Digital Marketing Specialist | MarketMasters | 2018-2021
• Increased lead generation 180% via LinkedIn ads
• Built content calendar driving 2M monthly visitors
• Analyzed 50+ campaigns identifying $200K savings

CERTIFICATIONS
Google Ads Certification | HubSpot Inbound Marketing
SEMrush SEO Toolkit | Facebook Blueprint

ACHIEVEMENTS
• Marketer of the Year 2023
• 300% ROI on Q4 campaign`
  }
};

// Add more templates as needed (data, manager, designer, entry)

function showPage(pageName) {
  pages.forEach(page => {
    document.getElementById(page).classList.remove('active');
    const btn = document.querySelector(`[onclick="showPage('${page}')"]`);
    if (btn) btn.classList.remove('active');
  });
  
  document.getElementById(pageName).classList.add('active');
  const activeBtn = document.querySelector(`[onclick="showPage('${pageName}')"]`);
  if (activeBtn) activeBtn.classList.add('active');
  currentPage = pageName;
  window.scrollTo({top: 0, behavior: 'smooth'});
  
  if (pageName === 'results' && localStorage.getItem('jobSnapScore')) {
    document.getElementById('resultsDiv').style.display = 'block';
    loadResults();
  }
}

function downloadTemplate(type) {
  const template = resumeTemplates[type] || resumeTemplates.software;
  const blob = new Blob([template.content], { type: 'text/plain' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `${template.name.replace(/ /g, '_')}.txt`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  
  // Show success message
  const status = document.getElementById('status');
  if (status) {
    status.textContent = `✅ ${template.name} downloaded! Edit and upload to analyze.`;
    setTimeout(() => { status.textContent = ''; }, 3000);
  }
}

const fileInput = document.getElementById('resume');
const statusText = document.getElementById('status');
const previewText = document.getElementById('filePreview');

fileInput.addEventListener('change', () => {
  const file = fileInput.files[0];
  if (!file) return;

  if (uploadedResumeURL) URL.revokeObjectURL(uploadedResumeURL);
  uploadedResumeURL = URL.createObjectURL(file);

  statusText.textContent = `✅ ${file.name} loaded successfully`;
  previewText.textContent = "👁️ Preview resume";
  previewText.onclick = () => window.open(uploadedResumeURL, '_blank');
  previewText.style.display = 'block';
});

async function checkSnap() {
  const file = fileInput.files[0];
  if (!file) {
    statusText.textContent = "❌ Please upload a resume first";
    return;
  }

  statusText.textContent = "🔍 Analyzing your resume...";
  try {
    const text = await extractText(file);
    const score = analyzeSnap(text);
    showResults(score);
    showPage('results');
  } catch (error) {
    statusText.textContent = "❌ Error analyzing resume. Please try again.";
    console.error(error);
  }
}

async function extractText(file) {
  if (file.name.toLowerCase().endsWith('.pdf')) {
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await pdfjsLib.getDocument(arrayBuffer).promise;
    let text = '';
    for (let i = 1; i <= pdf.numPages; i++) {
      const page = await pdf.getPage(i);
      const content = await page.getTextContent();
      text += content.items.map(item => item.str).join(' ') + ' ';
    }
    return text.toLowerCase();
  }
  return (await file.text()).toLowerCase();
}

function analyzeSnap(text) {
  let score = 25;
  const keywords = ['project', 'experience', 'skill', 'education', 'developed', 'built', 'created', 'led', 'team'];
  const skills = ['html', 'css', 'javascript', 'python', 'java', 'react', 'node', 'sql', 'git', 'aws'];
  const jobKeywords = ['software', 'developer', 'engineer', 'frontend', 'backend', 'fullstack', 'marketing', 'manager'];

  keywords.forEach(kw => { if (text.includes(kw)) score += 6; });
  skills.forEach(skill => { if (text.includes(skill)) score += 4; });
  jobKeywords.forEach(jobKw => { if (text.includes(jobKw)) score += 5; });

  return Math.min(score, 98);
}

function showResults(score) {
  localStorage.setItem('jobSnapScore', score);
  
  const scoreNumber = document.getElementById('scoreNumber');
  const scoreCircle = document.getElementById('scoreCircle');
  const scoreTitle = document.getElementById('scoreTitle');
  
  scoreNumber.textContent = score + '%';
  scoreCircle.style.background = `conic-gradient(#10b981 0deg ${score * 3.6}deg, #e5e7eb ${score * 3.6}deg 360deg)`;
  
  if (score >= 85) {
    scoreTitle.textContent = '🎉 Excellent JobSnap!';
    scoreTitle.style.color = '#10b981';
  } else if (score >= 70) {
    scoreTitle.textContent = '👍 Good JobSnap!';
    scoreTitle.style.color = '#f59e0b';
  } else {
    scoreTitle.textContent = '📈 Improve Your Snap';
    scoreTitle.style.color = '#ef4444';
  }
}

function loadResults() {
  const score = localStorage.getItem('jobSnapScore');
  if (score) showResults(parseInt(score));
}

document.addEventListener('DOMContentLoaded', () => {
  loadResults();
});
</script>
</body>
</html>
