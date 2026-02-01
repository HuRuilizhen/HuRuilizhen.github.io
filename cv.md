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
                    Undergraduate Student Teaching Fellowship (USTF)
                    <ul class="nested-list">
                        <li>CSC1003 - Introduction to Computer Science and Java Programming (Autumn 2023)</li>
                        <li>CSC1003 - Introduction to Computer Science and Java Programming (Autumn 2024)</li>
                        <li>CSC2003 - Introduction to Java Programming (Spring 2024)</li>
                        <li>CSC3170 - Database System (Autumn 2025)</li>
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
        <h3>The Jianchen Science and Technology Ltd.<span>Jul. 2024 - Jan. 2025</span></h3>
        <h4>
            <p>Co-founder &amp; Full Stack Developer</p>
            <p>Shenzhen, Guangdong</p>
        </h4>
        <ul>
            <li>Co-founded a student startup focused on automating text revision workflows for the publishing industry.</li>
            <li>Contributed to the construction of an AI-assisted editorial correction system, including data acquisition from publishers and preparation of annotated PDF datasets containing correction marks for OCR model fine-tuning.</li>
            <li>Designed and implemented a task scheduling and orchestration module to support asynchronous and batch processing of large-scale PDF document correction jobs.</li>
            <li>Developed the company official website and implemented the majority of business-facing front-end and back-end functionalities (excluding algorithm development), enabling end-to-end integration between OCR models and downstream vision workflows.</li>
            <li>Built front-end interfaces using <strong>Vue.js</strong> and back-end services with <strong>Flask</strong>, supporting real-world publisher trials and internal production workflows.</li>
        </ul>
        <h3>Tencent Holdings Limited – Weixin Group<span>May. 2025 - Sep. 2025</span></h3>
        <h4>
            <p>PC Client Developer Intern</p>
            <p>Guangzhou, Guangdong</p>
        </h4>
        <ul>
            <li>Developed cross-platform desktop modules using <strong>Qt</strong> and <strong>C++</strong>, interfacing with native Windows and macOS APIs to ensure consistent client behavior.</li>
            <li>Optimized build and link workflows in a large-scale C++ codebase using <strong>Clang</strong> and <strong>Bazel</strong>, contributing to modularization and reducing incremental build latency.</li>
            <li>Investigated and resolved complex runtime issues in multi-threaded components; improved code stability and debuggability through systematic profiling and debugging practices.</li>
        </ul>
    </div>
    <div class="cv-section">
        <h2>Research Experience</h2>
        <h3>Research on Directed Acyclic Graph Scheduling Algorithms<span>Nov. 2023 - Jul. 2024</span></h3>
        <h4>
            <p>Supervised by <a href="https://math4ai.cuhk.edu.cn/en/teacher/41">Prof. Wenye Li</a>; Experimentation Lead</p>
            <p>Shenzhen, Guangdong</p>
        </h4>
        <ul>
            <li>Studied scheduling algorithms for directed acyclic graphs (DAGs), with a focus on approximate dynamic programming approaches.</li>
            <li>Led the replication, maintenance, and extension of experimental implementations from prior literature, and conducted large-scale data collection and performance evaluation.</li>
            <li>Co-authored the paper <strong>An Approximate Dynamic Programming Method for Directed Acyclic Graph</strong>, accepted by ICONIP 2024 (Auckland, New Zealand) (<a href="https://link.springer.com/chapter/10.1007/978-981-96-6954-7_15">paper</a>).</li>
            <li>Served as the primary technical contributor for a patent application on a novel DAG scheduling method and system (CN202410923163.0, <a href="https://xueshu.baidu.com/usercenter/paper/show?paperid=170p0gh0rd580ry0ey610cx0xj149157&svcp_stk=1_3Lg3MlSxXq_HNUFUhQPF_Y29cZqwQ8FpkC0fOxy6BcSD8LYBTinB-J1v97o0rQszVnw_AiaGO5yaKUm5vpYhtS-4apP8kFKCtEHUqAEYbzquabDxYP05QjJu10-CfRXn6hZFWeaGWnEQT4dy-R9YLgV17AKk6JdoS1pN1gzrx7IF4p7SSyr0S71zxeRBH2xh">patent</a>).</li>
            <li>Maintained an open-source experimental codebase to support reproducibility and benchmarking (<a href="https://github.com/HuRuilizhen/NN-to-DAG/">code</a>).</li>
        </ul>
        <h3>Research on Kolmogorov-Arnold Auto-Encoder for Representation Learning<span>Sep. 2024 - Jan. 2025</span></h3>
        <h4>
            <p>Supervised by <a href="https://math4ai.cuhk.edu.cn/en/teacher/41">Prof. Wenye Li</a>; Research Contributor (Method &amp; Experiments)</p>
            <p>Shenzhen, Guangdong</p>
        </h4>
        <ul>
            <li>Proposed the Kolmogorov-Arnold Auto-Encoder (KAE), integrating Kolmogorov-Arnold Networks (KAN) with auto-encoders to enhance representation learning.</li>
            <li>Designed and conducted extensive experiments demonstrating improved latent representation quality, reduced reconstruction error, and superior performance on retrieval, classification, and denoising tasks.</li>
            <li>Co-authored the paper <strong>KAE: Kolmogorov-Arnold Auto-Encoder for Representation Learning</strong> (<a href="https://arxiv.org/abs/2501.00420">paper</a>).</li>
            <li>Contributed to the development and public release of an open-source implementation to ensure reproducibility (<a href="https://github.com/SciYu/KAE">code</a>).</li>
        </ul>
    </div>
    <div class="cv-section">
        <h2>Publications & Patents</h2>
        <div class="publication-item">
            <div class="publication-title">An Approximate Dynamic Programming Method for Directed Acyclic Graph</div>
            <div class="publication-authors">Yuqi Ma, <strong>Ruilizhen Hu</strong>, Ruixuan Qi, Jianfeng Mao, and Wenye Li</div>
            <div class="publication-venue">ICONIP 2024 (Accepted)</div>
            <div class="publication-links">
                <a href="https://link.springer.com/chapter/10.1007/978-981-96-6954-7_15">Springer</a>
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
            <div class="publication-title">A Scheduling Method and Related Equipment Applied to Directed Acyclic Graph</div>
            <div class="publication-authors"><strong>Ruilizhen Hu</strong>, Yuqi Ma, Wenye Li, Yungbin Zhao</div>
            <div class="publication-venue">China National Intellectual Property Administration</div>
            <div class="publication-links">
                <a href="https://patents.google.com/patent/CN118656215A/en">Patent Link</a>
            </div>
        </div>
        <div class="publication-item">
            <div class="publication-title">File Reconstruction Method, System, Electronic Device and Medium Based on Machine Learning</div>
            <div class="publication-authors"><strong>Ruilizhen Hu</strong>, Bohan Wu, Ziyue Cai, Chuxuan Jia, Haonan Xue, Yan Ke</div>
            <div class="publication-venue">China National Intellectual Property Administration</div>
            <div class="publication-links">
                <a href="https://patents.google.com/patent/CN120635917A/en">Patent Link</a>
            </div>
        </div>
    </div>
    <div class="cv-section">
        <h2>Technical Skills</h2>
        <ul>
            <li>Operating Systems: Ubuntu, macOS</li>
            <li>Markup Languages: HTML5, CSS3, Markdown, LaTeX</li>
            <li>Programming Languages: Java, JavaScript, TypeScript, C++, Python, Rust</li>
            <li>Development Frameworks: Vue.js, React.js, Next.js, Nest.js, Flask, Django</li>
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
            <p>Weihai, Shandong, China</p>
        </h4>
        <h3>
            Dean's List, SDS/FE Programme <span>Sep. 2024</span>
        </h3>
        <h4>
            <p>School of Data Science, The Chinese University of Hong Kong, Shenzhen</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
        <h3>
            Dean's List, SDS/FE Programme <span>Sep. 2025</span>
        </h3>
        <h4>
            <p>School of Data Science, The Chinese University of Hong Kong, Shenzhen</p>
            <p>Shenzhen, Guangdong, China</p>
        </h4>
    </div>
</div>

> or, if you're old-school, you can still download the PDF version <a href="https://drive.google.com/file/d/11NBcbhpz-SWUbhM0L7TXcR34u68qVq3I/view?usp=drive_link">here</a>!
