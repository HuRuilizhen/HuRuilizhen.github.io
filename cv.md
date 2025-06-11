---
layout: page
title: "Curriculum Vitae"
description: "A CV Page of Author"
---

<style>
.cv-header {
    width: 100%;
    display: flex;
    align-items: center;
    gap: 20px;
    padding: 20px;
    background: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 3px #ccc;
}
.cv-header-text {
    flex: 1;
}
.cv-header img {
    width: 200px;
    object-fit: cover;
}
.cv-header-text h1 {
    color: #2c3e50;
    font-size: 24px;
    margin: 0;
}
.cv-header-text p {
    color: #7f8c8d;
    margin: 5px 0;
}
.cv-header-text p strong {
    color: #2c3e50;
    font-weight: bold;
}
.cv-container {
    margin-top: 20px;
    margin-bottom: 20px;
}
.cv-section {
    width: 100%;
    margin-bottom: 20px;
    background: #f9f9f9;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 2px 3px #ccc;
}
.cv-section h2 {
    color: #2c3e50;
    border-bottom: 2px solid #c73b45;
    padding-bottom: 5px;
}
.cv-section h3 {
    color: #2c3e50;
    margin-bottom: 5px;
    font-size: 18px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.cv-section h3 span {
    padding-top: 5px;
    font-size: 14px;
    color: #7f8c8d;
    margin-left: 10px;
}
.cv-section h4 {
    margin-top: 5px;
    margin-bottom: 5px;
    font-size: 14px;
    font-weight: normal;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.cv-section p {
    color: #7f8c8d;
    margin: 0px;
}
.cv-section ul {
    padding-left: 20px;
    list-style: none;
}
.cv-section ul li::before {
    content: "\2022";
    color: #c73b45;
    font-weight: bold;
    display: inline-block;
    width: 1em;
    margin-left: -1em;
}
.publication-item {
    margin-bottom: 15px;
    padding-left: 20px;
    position: relative;
}
.publication-item::before {
    content: "\2022";
    color: #c73b45;
    position: absolute;
    left: 0;
}
.publication-title {
    font-weight: 500;
    color: #2c3e50;
}
.publication-authors {
    color: #7f8c8d;
    font-size: 0.95em;
}
.publication-venue {
    font-style: italic;
    color: #7f8c8d;
}
.publication-links a {
    margin-right: 12px;
    text-decoration: none;
}
.nested-list {
    padding-left: 25px;
    list-style: none;
}
</style>

<div class="cv-header">
    <div class="cv-header-text">
        <h1>Ruilizhen (Ray) Hu 胡瑞李蓁</h1>
        <p><strong>Email:</strong> <a href="mailto:huruilizhen@gmail.com">huruilizhen@gmail.com</a></p>
        <p><strong>LinkedIn:</strong> <a href="https://www.linkedin.com/in/ruilizhen-hu">linkedin.com/in/ruilizhen-hu</a></p>
        <p><strong>GitHub:</strong> <a href="https://github.com/HuRuilizhen">github.com/HuRuilizhen</a></p>
        <p><strong>Website:</strong> <a href="https://huruilizhen.github.io">huruilizhen.github.io</a></p>
        <p><strong>Twitter:</strong> <a href="https://x.com/RuilizhenH">x.com/RuilizhenH</a></p>
        <p><strong>Telegram:</strong> <a href="https://t.me/HuRuilizhen">@HuRuilizhen</a></p>
    </div>
    <img src="/images/author.jpeg" alt="Ruilizhen Hu">
</div>

<div class="cv-container">
    <div class="cv-section">
        <h2>Education</h2>
            <h3>The Chinese University of Hong Kong, Shenzhen Campus<span>Aug. 2022 - Jun. 2026</span></h3>
            <h4>
                <p>Bachelor of Science in Computer Science and Technology</p>
                <p>Shenzhen, Guangdong, China</p>
            </h4>
            <ul>
                <li>Academic Standing Top 8.5% (11th/129 in MGPA Ranking)</li>
                <li>
                    Under Graduate Teaching Fellowship
                    <ul class="nested-list">
                        <li>CSC1003 Introduction to Computer Science and Java Programming (Autumn 2023)</li>
                        <li>CSC1003 Introduction to Computer Science and Java Programming (Autumn 2024)</li>
                        <li>CSC2003 Itroduction to Java Programming (Spring 2024)</li>
                    </ul>
                </li>
            </ul>
            <h3>Nanyang Technological University<span>Jan. 2025 - May. 2025</span></h3>
            <h4>
                <p>Exchange Program in Computer Science / Electrical and Electronic Engineering</p>
                <p>Singapore</p>
            </h4>
    </div>
    <div class="cv-section">
        <h2>Work Experience</h2>
        <h3>
            Shenzhen Zhongzhilechuang Technology Co., Ltd.
            <span>Nov. 2022 - Jun. 2024</span>
        </h3>
        <h4>
            <p>Informatics Competition Instructor (Part-time Contract)</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <ul>
            <li>Mentored 15+ middle / high school students in algorithm design and programming, achieving:
                <ul class="nested-list">
                    <li>3× First Prize (CSP-J/S 2023 Second Round)</li>
                    <li>5× Second Prize (CSP-J/S 2023 Second/Advanced Round)</li>
                </ul>
            </li>
            <li>Designed and deployed a Django-based educational blog platform (<a href="http://blogweb.fun">blogweb.fun</a>):
                <ul class="nested-list">
                    <li>Implemented user authentication, content management, and commenting system</li>
                    <li>Optimized page load speed by 40% through caching strategies</li>
                    <li>Open-source codebase: <a href="https://github.com/HuRuiilzhen/Blog-Web">github.com/HuRuiilzhen/Blog-Web</a></li>
                </ul>
            </li>
        </ul>
        <h3>The Jianchen Science and Technology Ltd.<span>Jul. 2024 - Present</span></h3>
        <h4>
            <p>Full Stack Developer (Intern)</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <ul>
        <li>Led development of enterprise SaaS platform using Vue.js/Flask stack:
            <ul class="nested-list">
                <li>Built dynamic dashboard with real-time data visualization (ECharts integration)</li>
                <li>Designed RESTful APIs handling 1k+ RPM with 99.9% uptime</li>
            </ul>
        </li>
        <li>DevOps improvements:
            <ul class="nested-list">
                <li>Reduced CI/CD pipeline runtime from 12min → 4min through parallel testing</li>
                <li>Implemented automated monitoring with Prometheus/Grafana</li>
            </ul>
        </li>
        <li>Key contributor to architecture redesign:
            <ul class="nested-list">
                <li>Migrated monolithic backend to microservices (Docker/Kubernetes)</li>
                <li>Improved API response time by 35% with Redis caching</li>
            </ul>
        </li>
    </ul>
    <h3>Tencent Holdings Limited<span>May. 2025 - Sep. 2025 (Expected)</span></h3>
    <h4>
        <p>Client-side Developer Intern (WXG)</p>
        <p>Guangzhou, Guangdong, China</p>
    </h4>
    <ul>
        <li>Will be working on the PC client-side development of WeCom using C++ with duilib and Qt</li>
        <li>Expected to contribute to the development of new features and bug fixing</li>
    </ul>
    </div>
    <div class="cv-section">
        <h2>Research Experience</h2>
        <h3>Directed Acyclic Graph Scheduling Algorithms<span>Nov. 2023 - Jul. 2024</span></h3>
        <h4>
            <p>Supervised by <a href="https://www.sribd.cn/en/teacher/20">Prof. Wenye Li</a></p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <ul>
            <li>Replicated and maintained NN-to-DAG repository from academic papers</li>
            <li>Developed novel dynamic programming-based scheduling algorithm</li>
            <li>Led data collection/analysis for algorithm validation</li>
        </ul>
        <h3>Kolmogorov-Arnold Auto-Encoder for Representation Learning<span>Aug. 2024 - Oct. 2024</span></h3>
        <h4>
            <p>Supervised by <a href="https://www.sribd.cn/en/teacher/20">Prof. Wenye Li</a></p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <ul>
            <li>Proposed hybrid architecture combining KAN networks with autoencoders</li>
            <li>Conducted comparative experiments on latent representation quality</li>
            <li>Implemented open-source framework for reproducibility</li>
        </ul>
    </div>
    <div class="cv-section">
        <h2>Publications & Patents</h2>
        <div class="publication-item">
            <div class="publication-title">An Approximate Dynamic Programming Method for Directed Acyclic Graph</div>
            <div class="publication-authors">Yuqi Ma, <strong>Ruilizhen Hu</strong>, Ruixuan Qi, Jianfeng Mao, and Wenye Li</div>
            <div class="publication-venue">ICONIP 2024 (Accepted)</div>
            <div class="publication-links">
                <a href="#">Preprint</a>
                <a href="https://github.com/HuRuilizhen/NN-to-DAG">Code</a>
            </div>
        </div>
        <div class="publication-item">
            <div class="publication-title">KAE: Kolmogorov-Arnold Auto-Encoder for Representation Learning</div>
            <div class="publication-authors">Fangchen Yu, <strong>Ruilizhen Hu</strong>, Yidong Lin, Yuqi Ma, Zhenghao Huang, Wenye Li</div>
            <div class="publication-venue">arXiv preprint (Submitted)</div>
            <div class="publication-links">
                <a href="https://arxiv.org/abs/2501.00420">arXiv</a>
                <a href="https://github.com/SciYu/KAE">Code</a>
            </div>
        </div>
        <div class="publication-item">
            <div class="publication-title">A Novel Scheduling Method and Associated Equipment for Directed Acyclic Graphs</div>
            <div class="publication-authors"><strong>Ruilizhen Hu</strong>, Yuqi Ma, Wenye Li, Yungbin Zhao</div>
            <div class="publication-venue">China National Intellectual Property Administration</div>
            <div class="publication-links">
                <a href="https://xueshu.baidu.com/usercenter/paper/show?paperid=170p0gh0rd580ry0ey610cx0xj149157">Patent Link</a>
            </div>
        </div>
    </div>
    <div class="cv-section">
        <h2>Technical Skills</h2>
        <ul>
            <li>Operating Systems: Ubuntu, Mac OS</li>
            <li>Markup Languages: HTML5, CSS3, Markdown, LaTeX</li>
            <li>Programming Languages: Java, JavaScript, C++, Python, Rust</li>
            <li>Development Frameworks: Vue.js, Flask, Django</li>
            <li>Database Systems: MongoDB, PostgreSQL, MySQL, SQLite</li>
            <li>Version Control: Git</li>
            <li>Hosting Tools: Heroku, Alibaba Cloud</li>
            <li>Testing Tools: Postman</li>
            <li>Machine Learning: TensorFlow, PyTorch</li>
        </ul>
    </div>
    <div class="cv-section">
        <h2>Awards & Recognition</h2>
        <h3>
            Bronze Medal, National Olympiad in Informatics (NOI) <span>Aug. 2021</span>
        </h3>
        <h4>
            <p>China Computer Federation</p>
            <p>Yuyao, Zhejiang, China</p>
        </h4>
        <h3>
            Full Admission Scholarship <span>Jun. 2022</span>
        </h3>
        <h4>
            <p>Muse College, The Chinese University of Hong Kong, Shenzhen</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <h3>
            Silver Medal, China Collegiate Programming Contest (CCPC) Weihai Station <span>Nov. 2022</span>
        </h3>
        <h4>
            <p>China Collegiate Programming Contest Committee</p>
            <p>Weihai, Shangdong, China</p>
        </h4>
        <h3>
            Dean's List, SDS/FE Programme <span>Sep. 2024</span>
        </h3>
        <h4>
            <p>School of Data Science, The Chinese University of Hong Kong, Shenzhen</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
    </div>
</div>

> or, if you're old-school, you can still download the PDF version <a href="https://drive.google.com/file/d/11NBcbhpz-SWUbhM0L7TXcR34u68qVq3I/view?usp=drive_link">here</a>!