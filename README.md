# biomass-combustion
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>การเผาไหม้ไม่สมบูรณ์ของชีวมวล | แบบจำลองอะตอมของนีลส์ โบร์</title>

    <!-- MathJax สำหรับสมการและสัญลักษณ์ทางวิทยาศาสตร์ -->
    <script>
        window.MathJax = {
            tex: {
                inlineMath: [['$', '$'], ['\\(', '\\)']],
                displayMath: [['$$', '$$'], ['\\[', '\\]']]
            },
            svg: {
                fontCache: 'global'
            }
        };
    </script>
    <script async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js"></script>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(145deg, #eef5ed, #dce8dc);
            font-family: "Segoe UI", Tahoma, Arial, sans-serif;
            color: #27352a;
            padding: 25px 15px;
            line-height: 1.7;
        }

        .container {
            max-width: 1150px;
            margin: auto;
            background: #fffdf8;
            border-radius: 30px;
            padding: 35px;
            box-shadow: 0 20px 50px rgba(35, 55, 35, 0.18);
        }

        /* HEADER */
        header {
            background: linear-gradient(135deg, #28452d, #527755);
            color: white;
            padding: 30px;
            border-radius: 25px;
            margin-bottom: 30px;
            box-shadow: 0 10px 25px rgba(30, 60, 35, 0.2);
        }

        header h1 {
            font-size: 2.35rem;
            margin-bottom: 10px;
            line-height: 1.3;
        }

        header p {
            font-size: 1.05rem;
            opacity: 0.92;
        }

        .tag {
            display: inline-block;
            background: #dcebcf;
            color: #29482e;
            padding: 5px 16px;
            border-radius: 30px;
            font-weight: bold;
            margin-top: 15px;
        }

        /* SECTION */
        section {
            margin-top: 35px;
        }

        section h2 {
            color: #315b38;
            border-left: 7px solid #70966c;
            padding-left: 14px;
            margin-bottom: 18px;
            font-size: 1.65rem;
        }

        section h3 {
            color: #466b48;
            margin: 22px 0 10px;
        }

        /* EQUATION */
        .equation-box {
            background: #1e2e21;
            color: #f5f7e9;
            border-radius: 20px;
            padding: 25px 15px;
            text-align: center;
            margin: 20px 0;
            box-shadow: inset 0 -5px 0 #77936c,
                        0 12px 25px rgba(0,0,0,0.15);
        }

        .equation {
            font-size: 1.65rem;
            font-weight: 600;
            overflow-x: auto;
            padding: 8px;
        }

        .equation-note {
            color: #c9d9bf;
            font-size: 0.9rem;
            margin-top: 8px;
        }

        .warning {
            background: #fff5d8;
            border-left: 6px solid #d6a83e;
            padding: 15px 18px;
            border-radius: 12px;
            margin: 18px 0;
        }

        /* GRID */
        .grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
        }

        .card {
            background: #f8faf5;
            border: 1px solid #dce5d7;
            border-radius: 20px;
            padding: 22px;
            box-shadow: 0 6px 15px rgba(50,70,50,0.08);
        }

        .card h3 {
            margin-top: 0;
            font-size: 1.35rem;
        }

        .card ul {
            padding-left: 22px;
        }

        .card li {
            margin-bottom: 6px;
        }

        .highlight {
            background: #e4efdf;
            color: #2e4d32;
            padding: 2px 9px;
            border-radius: 7px;
            font-weight: 600;
        }

        /* NUCLEAR CARDS */
        .atom-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-top: 20px;
        }

        .atom-card {
            background: linear-gradient(145deg, #f6faf3, #eaf2e7);
            border: 2px solid #cdddc9;
            border-radius: 22px;
            padding: 25px 18px;
            text-align: center;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .atom-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 25px rgba(50,80,50,0.15);
        }

        .atom-card h3 {
            margin: 0 0 10px;
            color: #315c38;
        }

        .nuclear-symbol {
            font-size: 2.2rem;
            font-weight: bold;
            color: #203d27;
            margin: 10px 0;
        }

        .atom-name {
            color: #637462;
            margin-bottom: 10px;
        }

        .particle-row {
            display: flex;
            justify-content: space-around;
            gap: 5px;
            margin: 15px 0;
        }

        .particle {
            background: white;
            border-radius: 12px;
            padding: 8px;
            flex: 1;
            border: 1px solid #d7e2d3;
        }

        .particle strong {
            display: block;
            font-size: 1.2rem;
            color: #315b38;
        }

        .bohr-config {
            background: #315b38;
            color: white;
            padding: 8px 12px;
            border-radius: 30px;
            display: inline-block;
            font-weight: 600;
        }

        .valence {
            margin-top: 12px;
            color: #53654f;
            font-size: 0.92rem;
        }

        /* BOHR DIAGRAM */
        .bohr-area {
            background: #f4f8f1;
            border-radius: 22px;
            padding: 25px;
            margin-top: 22px;
            border: 1px solid #d4dfd0;
        }

        .bohr-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 25px;
            margin-top: 20px;
        }

        .bohr-diagram {
            text-align: center;
        }

        .atom {
            width: 190px;
            height: 190px;
            margin: auto;
            position: relative;
            border-radius: 50%;
            background: radial-gradient(circle at center, #d6b36a 0 22%, #e8d8aa 23% 25%, transparent 26%);
            border: 2px solid #a9bca4;
        }

        .shell {
            position: absolute;
            border: 2px solid #8ca38a;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        .shell-k { width: 90px; height: 90px; }
        .shell-l { width: 150px; height: 150px; }

        .electron {
            position: absolute;
            width: 13px;
            height: 13px;
            background: #315b38;
            border: 2px solid white;
            border-radius: 50%;
            box-shadow: 0 1px 4px rgba(0,0,0,0.25);
        }

        /* C electrons: K2 L4 */
        .c-e1 { left: 88px; top: 43px; }
        .c-e2 { left: 88px; top: 133px; }
        .c-e3 { left: 88px; top: 13px; }
        .c-e4 { left: 88px; top: 163px; }
        .c-e5 { left: 13px; top: 88px; }
        .c-e6 { left: 163px; top: 88px; }

        /* H electron: K1 */
        .h-e1 { left: 88px; top: 43px; }

        /* O electrons: K2 L6 */
        .o-e1 { left: 88px; top: 43px; }
        .o-e2 { left: 88px; top: 133px; }
        .o-e3 { left: 88px; top: 13px; }
        .o-e4 { left: 88px; top: 163px; }
        .o-e5 { left: 13px; top: 88px; }
        .o-e6 { left: 163px; top: 88px; }
        .o-e7 { left: 35px; top: 35px; }
        .o-e8 { left: 140px; top: 140px; }

        .nucleus-label {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 0.8rem;
            font-weight: bold;
            color: #604b22;
            z-index: 5;
            line-height: 1.2;
        }

        /* TABLE */
        .table-wrapper {
            overflow-x: auto;
            margin-top: 20px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(40,60,40,0.08);
        }

        table {
            width: 100%;
            min-width: 800px;
            border-collapse: collapse;
            background: white;
        }

        th {
            background: #315b38;
            color: white;
            padding: 13px 10px;
            text-align: center;
        }

        td {
            padding: 13px 10px;
            border-bottom: 1px solid #dce5d8;
            text-align: center;
        }

        tr:nth-child(even) {
            background: #f4f8f1;
        }

        .symbol {
            font-size: 1.35rem;
            font-weight: bold;
        }

        /* FOOTER */
        footer {
            margin-top: 40px;
            padding-top: 20px;
            border-top: 2px solid #d3dfce;
            text-align: center;
            color: #657361;
            font-size: 0.9rem;
        }

        /* RESPONSIVE */
        @media (max-width: 850px) {
            .container { padding: 20px; }
            .grid, .atom-grid, .bohr-grid { grid-template-columns: 1fr; }
            header h1 { font-size: 1.8rem; }
            .equation { font-size: 1.2rem; }
            .atom { width: 180px; height: 180px; }
        }
    </style>
</head>
<body>

<div class="container">
    <!-- HEADER -->
    <header>
        <h1>♻️ การเผาไหม้ไม่สมบูรณ์ของขยะชีวมวล</h1>
        <p>ศึกษาสมการเคมี องค์ประกอบของธาตุ ข้อมูลนิวเคลียร์ และแบบจำลองอะตอมของนีลส์ โบร์</p>
        <span class="tag">หัวข้อ 3.1 – 3.4</span>
    </header>

    <!-- สมการเคมี -->
    <section>
        <h2>🧪 สมการเคมีที่เลือกศึกษา</h2>
        <div class="equation-box">
            <div class="equation">
                $$\mathrm{C_6H_{10}O_5(s) + 3O_2(g) \rightarrow 6CO(g) + 5H_2O(g)}$$
            </div>
            <div class="equation-note">
                ตัวอย่างสมการการเผาไหม้ไม่สมบูรณ์ของเซลลูโลส 1 หน่วย
            </div>
        </div>

        <div class="warning">
            <strong>หมายเหตุทางวิทยาศาสตร์:</strong>
            สมการนี้เป็นแบบจำลองอย่างง่ายเพื่อแสดงการเกิด CO จากการเผาไหม้เซลลูโลสไม่สมบูรณ์ ในการเผาไหม้จริง ผลิตภัณฑ์และปริมาณที่เกิดขึ้นอาจแตกต่างกันตามอุณหภูมิ ปริมาณออกซิเจน ความชื้น และสภาวะของเตาเผา
        </div>
    </section>

    <!-- 3.1 -->
    <section>
        <h2>3.1 ส่วนประกอบของสมการเคมี</h2>
        <div class="grid">
            <div class="card">
                <h3>🌿 สารตั้งต้น (Reactants)</h3>
                <p><span class="highlight">$\mathrm{C_6H_{10}O_5}$</span> ใช้เป็นหน่วยตัวแทนของเซลลูโลสในชีวมวล (สถานะของแข็ง)</p>
                <p><span class="highlight">$\mathrm{O_2}$</span> แก๊สออกซิเจนที่ใช้ในการเผาไหม้ (ปริมาณจำกัด)</p>
                <ul>
                    <li>ประกอบด้วยธาตุคาร์บอน (C)</li>
                    <li>ประกอบด้วยธาตุไฮโดรเจน (H)</li>
                    <li>ประกอบด้วยธาตุออกซิเจน (O)</li>
                </ul>
            </div>

            <div class="card">
                <h3>🔥 ผลิตภัณฑ์ (Products)</h3>
                <p><span class="highlight">$\mathrm{CO}$</span> แก๊สคาร์บอนมอนอกไซด์ เกิดจากการเผาไหม้ไม่สมบูรณ์</p>
                <p><span class="highlight">$\mathrm{H_2O}$</span> ไอน้ำที่เกิดขึ้นจากปฏิกิริยา</p>
                <ul>
                    <li>อัตราส่วนอะตอมฝั่งซ้ายและขวาเท่ากันตามกฎทรงมวล</li>
                    <li>มีการดัดแปรอัตราส่วนออกซิเจนทำให้เกิดก๊าซพิษ $\mathrm{CO}$</li>
                </ul>
            </div>
        </div>
    </section>

    <!-- 3.2 -->
    <section>
        <h2>3.2 ข้อมูลนิวเคลียร์และอนุภาคพื้นฐาน</h2>
        <div class="atom-grid">
            <!-- Carbon -->
            <div class="atom-card">
                <h3>คาร์บอน (Carbon)</h3>
                <div class="nuclear-symbol">$^{12}_{6}\mathrm{C}$</div>
                <div class="atom-name">เลขอะตอม 6 | เลขมวล 12</div>
                <div class="particle-row">
                    <div class="particle">โปรตอน<strong>6</strong></div>
                    <div class="particle">นิวตรอน<strong>6</strong></div>
                    <div class="particle">อิเล็กตรอน<strong>6</strong></div>
                </div>
                <div class="bohr-config">การจัดเรียง: 2, 4</div>
                <div class="valence">เวเลนซ์อิเล็กตรอน = 4</div>
            </div>

            <!-- Hydrogen -->
            <div class="atom-card">
                <h3>ไฮโดรเจน (Hydrogen)</h3>
                <div class="nuclear-symbol">$^{1}_{1}\mathrm{H}$</div>
                <div class="atom-name">เลขอะตอม 1 | เลขมวล 1</div>
                <div class="particle-row">
                    <div class="particle">โปรตอน<strong>1</strong></div>
                    <div class="particle">นิวตรอน<strong>0</strong></div>
                    <div class="particle">อิเล็กตรอน<strong>1</strong></div>
                </div>
                <div class="bohr-config">การจัดเรียง: 1</div>
                <div class="valence">เวเลนซ์อิเล็กตรอน = 1</div>
            </div>

            <!-- Oxygen -->
            <div class="atom-card">
                <h3>ออกซิเจน (Oxygen)</h3>
                <div class="nuclear-symbol">$^{16}_{8}\mathrm{O}$</div>
                <div class="atom-name">เลขอะตอม 8 | เลขมวล 16</div>
                <div class="particle-row">
                    <div class="particle">โปรตอน<strong>8</strong></div>
                    <div class="particle">นิวตรอน<strong>8</strong></div>
                    <div class="particle">อิเล็กตรอน<strong>8</strong></div>
                </div>
                <div class="bohr-config">การจัดเรียง: 2, 6</div>
                <div class="valence">เวเลนซ์อิเล็กตรอน = 6</div>
            </div>
        </div>
    </section>

    <!-- 3.3 -->
    <section>
        <h2>3.3 แบบจำลองอะตอมของนีลส์ โบร์ (Niels Bohr Model)</h2>
        <div class="bohr-area">
            <p>แสดงระดับชั้นพลังงานของอิเล็กตรอน (K, L) ล้อมรอบนิวเคลียสของแต่ละธาตุ:</p>
            <div class="bohr-grid">
                <!-- Carbon Diagram -->
                <div class="bohr-diagram">
                    <h4>คาร์บอน (C)</h4>
                    <div class="atom">
                        <div class="nucleus-label">6P<br>6N</div>
                        <div class="shell shell-k"></div>
                        <div class="shell shell-l"></div>
                        <!-- K shell -->
                        <div class="electron c-e1"></div>
                        <div class="electron c-e2"></div>
                        <!-- L shell -->
                        <div class="electron c-e3"></div>
                        <div class="electron c-e4"></div>
                        <div class="electron c-e5"></div>
                        <div class="electron c-e6"></div>
                    </div>
                </div>

                <!-- Hydrogen Diagram -->
                <div class="bohr-diagram">
                    <h4>ไฮโดรเจน (H)</h4>
                    <div class="atom">
                        <div class="nucleus-label">1P<br>0N</div>
                        <div class="shell shell-k"></div>
                        <!-- K shell -->
                        <div class="electron h-e1"></div>
                    </div>
                </div>

                <!-- Oxygen Diagram -->
                <div class="bohr-diagram">
                    <h4>ออกซิเจน (O)</h4>
                    <div class="atom">
                        <div class="nucleus-label">8P<br>8N</div>
                        <div class="shell shell-k"></div>
                        <div class="shell shell-l"></div>
                        <!-- K shell -->
                        <div class="electron o-e1"></div>
                        <div class="electron o-e2"></div>
                        <!-- L shell -->
                        <div class="electron o-e3"></div>
                        <div class="electron o-e4"></div>
                        <div class="electron o-e5"></div>
                        <div class="electron o-e6"></div>
                        <div class="electron o-e7"></div>
                        <div class="electron o-e8"></div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 3.4 -->
    <section>
        <h2>3.4 ตารางสรุปเปรียบเทียบคุณสมบัติทางอะตอม</h2>
        <div class="table-wrapper">
            <table>
                <thead>
                    <tr>
                        <th>ชื่อธาตุ</th>
                        <th>สัญลักษณ์</th>
                        <th>สัญลักษณ์นิวเคลียร์</th>
                        <th>จำนวนโปรตอน</th>
                        <th>จำนวนนิวตรอน</th>
                        <th>การจัดเรียงอิเล็กตรอน</th>
                        <th>เวเลนซ์อิเล็กตรอน</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>คาร์บอน</td>
                        <td class="symbol">C</td>
                        <td>$^{12}_{6}\mathrm{C}$</td>
                        <td>6</td>
                        <td>6</td>
                        <td>2, 4</td>
                        <td>4</td>
                    </tr>
                    <tr>
                        <td>ไฮโดรเจน</td>
                        <td class="symbol">H</td>
                        <td>$^{1}_{1}\mathrm{H}$</td>
                        <td>1</td>
                        <td>0</td>
                        <td>1</td>
                        <td>1</td>
                    </tr>
                    <tr>
                        <td>ออกซิเจน</td>
                        <td class="symbol">O</td>
                        <td>$^{16}_{8}\mathrm{O}$</td>
                        <td>8</td>
                        <td>8</td>
                        <td>2, 6</td>
                        <td>6</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>

    <!-- FOOTER -->
    <footer>
        <p>สื่อการเรียนรู้แบบโต้ตอบ รายวิชาเคมี/วิทยาศาสตร์กายภาพ เรื่องการเผาไหม้ชีวมวลและโครงสร้างอะตอม</p>
    </footer>
</div>

</body>
</html>
