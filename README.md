<html lang="vi" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hành Trình Kiến Tạo & Phát Triển Chiến Tướng GHN</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@3.7.1/dist/chart.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css">

    <!-- Chosen Palette: Warm Neutrals & GHN Orange Accent -->
    <!-- Application Structure Plan: The SPA is architected as a comprehensive, top-down narrative journey called "Hành Trình Kiến Tạo Chiến Tướng GHN". It logically guides the user from the "Why" (Triết Lý & Tầm Nhìn) to the "What" (Khung Năng Lực & Hồ Sơ), the "How" (Hành Trình Thi Tuyển & Thử Thách), and finally the "Outcome" (Tiêu Chí Đánh Giá & Lộ Trình Phát Triển). This narrative structure synthesizes multiple complex reports into a single, digestible, and engaging flow, making it far more effective for communication and understanding than presenting disparate documents. -->
    <!-- Visualization & Content Choices: 
        - Competency Framework -> Goal: Inform & Compare -> Viz: Interactive Radar Charts (Chart.js) -> Interaction: Hover tooltips for definitions -> Justification: Radar charts are ideal for displaying multidimensional competency data in a compact, visually comparable format, making the integrated GHN framework (based on Korn Ferry, DDI) easy to grasp.
        - Competition Process -> Goal: Organize & Show Change -> Viz: Custom Vertical Timeline (HTML/CSS) -> Interaction: Click to expand details for each stage -> Justification: An interactive timeline simplifies the complex 2-day competition format, making the sequence, activities, and objectives of each round intuitive and engaging.
        - Evaluation Weighting -> Goal: Inform & Compare -> Viz: Donut Chart (Chart.js) -> Interaction: Hover to see percentages -> Justification: A donut chart clearly and modernly represents the proportional importance of each assessment module (Self-assessment, Case Study, Group/Interview), ensuring transparency.
        - Evaluation Criteria -> Goal: Organize & Inform -> Viz: Accordion/Collapsible Cards (HTML/JS) -> Interaction: Click to expand/collapse details -> Justification: This keeps the UI clean and allows users to drill down into specifics (like rating scales and calibration) without being overwhelmed, promoting progressive disclosure of information.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
            color: #1f2937;
        }
        .ghn-orange { color: #E84D1A; }
        .bg-ghn-orange { background-color: #E84D1A; }
        .border-ghn-orange { border-color: #E84D1A; }
        .nav-link {
            position: relative;
            transition: color 0.3s ease;
        }
        .nav-link::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -4px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #E84D1A;
            transition: width 0.3s ease;
        }
        .nav-link.active, .nav-link:hover {
            color: #E84D1A;
        }
        .nav-link.active::after, .nav-link:hover::after {
            width: 100%;
        }
        .chart-container {
            position: relative;
            margin: auto;
            height: 280px;
            width: 100%;
            max-width: 350px;
        }
        @media (min-width: 768px) {
            .chart-container { height: 320px; max-width: 400px; }
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            left: 1.25rem;
            top: 2.5rem;
            width: 2px;
            height: calc(100%);
            background-color: #e5e7eb;
            transform: translateX(-50%);
        }
        .timeline-item:last-child::before {
            display: none;
        }
        .timeline-dot {
            position: absolute;
            left: 1.25rem;
            top: 1rem;
            width: 24px;
            height: 24px;
            transform: translateX(-50%);
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-in-out;
        }
        .fade-in {
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInAnimation 0.8s ease-out forwards;
        }
        @keyframes fadeInAnimation {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        [x-cloak] { display: none !important; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800">

    <header id="header" class="bg-white/90 backdrop-blur-lg sticky top-0 z-50 shadow-md transition-all duration-300">
        <div class="container mx-auto px-6 py-4">
            <div class="flex justify-between items-center">
                <a href="#" class="text-2xl font-bold ghn-orange tracking-wider">CHIẾN TƯỚNG GHN</a>
                <nav class="hidden md:flex space-x-6 lg:space-x-8 text-sm font-semibold">
                    <a href="#philosophy" class="nav-link text-slate-600">Triết Lý</a>
                    <a href="#framework" class="nav-link text-slate-600">Khung Năng Lực</a>
                    <a href="#journey" class="nav-link text-slate-600">Hành Trình</a>
                    <a href="#challenge" class="nav-link text-slate-600">Thử Thách</a>
                    <a href="#criteria" class="nav-link text-slate-600">Tiêu Chí</a>
                    <a href="#roadmap" class="nav-link text-slate-600">Lộ Trình</a>
                </nav>
                <div class="md:hidden">
                    <button id="menu-btn" class="text-slate-700 focus:outline-none">
                        <i class="fas fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>
        <div id="mobile-menu" class="md:hidden hidden bg-white border-t">
            <a href="#philosophy" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Triết Lý</a>
            <a href="#framework" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Khung Năng Lực</a>
            <a href="#journey" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Hành Trình</a>
            <a href="#challenge" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Thử Thách</a>
            <a href="#criteria" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Tiêu Chí</a>
            <a href="#roadmap" class="block py-3 px-6 text-sm text-slate-700 hover:bg-slate-100">Lộ Trình</a>
        </div>
    </header>

    <main>
        <section id="hero" class="bg-white pt-24 pb-28">
            <div class="container mx-auto px-6 text-center">
                <h1 class="text-4xl md:text-6xl font-extrabold text-slate-900 leading-tight fade-in">Hành Trình Kiến Tạo Chiến Tướng GHN</h1>
                <p class="mt-6 text-lg md:text-xl text-slate-600 max-w-3xl mx-auto fade-in" style="animation-delay: 0.2s;">
                    Hệ thống đánh giá toàn diện, được xây dựng để xác định, phát triển và vinh danh những nhà lãnh đạo tương lai, những người sẽ dẫn dắt GHN chinh phục các đỉnh cao mới.
                </p>
                <div class="mt-12 fade-in" style="animation-delay: 0.4s;">
                    <a href="#philosophy" class="bg-ghn-orange text-white font-bold py-4 px-10 rounded-full hover:opacity-90 transition duration-300 shadow-lg hover:shadow-xl transform hover:scale-105">Khám Phá Ngay</a>
                </div>
            </div>
        </section>

        <section id="philosophy" class="py-24">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Triết Lý Cốt Lõi: Xây Dựng Hồ Sơ Năng Lực Toàn Diện</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Chúng tôi kiến tạo một hồ sơ năng lực đa chiều, vượt trên các chỉ số hiệu suất truyền thống để xác định những nhà lãnh đạo đích thực, dựa trên 3 trụ cột vững chắc.</p>
                </div>
                <div class="grid md:grid-cols-3 gap-8">
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 fade-in">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                            <i class="fa-solid fa-chart-line text-3xl ghn-orange"></i>
                        </div>
                        <h3 class="text-2xl font-bold text-slate-900 mb-3">Thành Tích Quá Khứ</h3>
                        <p class="text-slate-600">Đánh giá kết quả công việc và đóng góp đã được chứng minh qua các KPI vận hành và kinh doanh, là nền tảng của năng lực thực thi.</p>
                    </div>
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 fade-in" style="animation-delay: 0.2s;">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                            <i class="fa-solid fa-rocket text-3xl ghn-orange"></i>
                        </div>
                        <h3 class="text-2xl font-bold text-slate-900 mb-3">Tiềm Năng Lãnh Đạo</h3>
                        <p class="text-slate-600">Xác định khả năng phát triển trong tương lai thông qua các năng lực cốt lõi như Nhanh Nhẹn Học Hỏi (Learning Agility), Sáng Kiến và Tham Vọng.</p>
                    </div>
                    <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 fade-in" style="animation-delay: 0.4s;">
                        <div class="flex items-center justify-center h-16 w-16 rounded-full bg-ghn-orange/10 mb-6">
                            <i class="fa-solid fa-user-check text-3xl ghn-orange"></i>
                        </div>
                        <h3 class="text-2xl font-bold text-slate-900 mb-3">Lãnh Đạo Đích Thực</h3>
                        <p class="text-slate-600">Đánh giá phong cách lãnh đạo, mức độ tự nhận thức sâu sắc và khả năng truyền cảm hứng, nền tảng cho một nhà lãnh đạo bền vững.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="framework" class="py-24 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Khung Năng Lực Lãnh Đạo Tích Hợp</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Kết hợp các mô hình quốc tế (Korn Ferry, DDI) và bối cảnh đặc thù của GHN để xác định 4 cụm năng lực cốt lõi, là kim chỉ nam cho mọi hoạt động đánh giá và phát triển.</p>
                </div>
                <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="p-6 rounded-lg text-center fade-in"><div class="chart-container"><canvas id="chart1"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-slate-900">Tầm Nhìn & Tư Duy Chiến Lược</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.2s;"><div class="chart-container"><canvas id="chart2"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-slate-900">Lãnh Đạo & Tạo Ảnh Hưởng</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.4s;"><div class="chart-container"><canvas id="chart3"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-slate-900">Thực Thi & Hướng Đến Kết Quả</h3></div>
                    <div class="p-6 rounded-lg text-center fade-in" style="animation-delay: 0.6s;"><div class="chart-container"><canvas id="chart4"></canvas></div><h3 class="text-xl font-bold text-center mt-4 text-slate-900">Tự Nhận Thức & Thích Ứng</h3></div>
                </div>
            </div>
        </section>

        <section id="journey" class="py-24">
            <div class="container mx-auto px-6">
                <div class="text-center mb-20 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Hành Trình Thi Tuyển "Cơ Hội Cho Ai?"</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Một cuộc tranh tài kịch tính, minh bạch và công bằng trong 02 ngày, được thiết kế để tìm ra những ứng viên xuất sắc nhất.</p>
                </div>
                <div class="flex flex-col md:flex-row justify-center gap-12">
                    <!-- Day 1 -->
                    <div class="md:w-1/2 fade-in">
                        <h3 class="text-2xl font-bold text-center mb-8 ghn-orange">NGÀY 1: VÒNG TRANH TÀI - "DẤN THÂN & CHỨNG TỎ"</h3>
                        <div class="relative pl-12">
                            <div class="timeline-item pb-10">
                                <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-flag-checkered"></i></div>
                                <p class="text-sm text-slate-500">08:00 - 09:00</p>
                                <h4 class="font-bold text-lg mt-1">Khai mạc & Chia 5 Bảng đấu</h4>
                                <p class="text-slate-600 text-sm mt-1">20 "Chiến Tướng" ra mắt và bốc thăm chia 5 bảng (A, B, C, D, E) ngẫu nhiên.</p>
                            </div>
                            <div class="timeline-item pb-10">
                                <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-puzzle-piece"></i></div>
                                <p class="text-sm text-slate-500">09:00 - 12:00</p>
                                <h4 class="font-bold text-lg mt-1">Giải Mã Tình Huống Kinh Doanh</h4>
                                <p class="text-slate-600 text-sm mt-1">Các bảng đấu thảo luận (60'), trình bày (10') và phản biện (10') giải pháp cho một case study thực tế.</p>
                            </div>
                            <div class="timeline-item">
                                <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-trophy"></i></div>
                                <p class="text-sm text-slate-500">13:00 - 13:45</p>
                                <h4 class="font-bold text-lg mt-1">Công Bố Top 10</h4>
                                <p class="text-slate-600 text-sm mt-1">Hội đồng đánh giá chọn 2 ứng viên xuất sắc nhất từ mỗi bảng để đi tiếp vào Vòng 2.</p>
                            </div>
                        </div>
                    </div>
                    <!-- Day 2 -->
                    <div class="md:w-1/2 fade-in" style="animation-delay: 0.2s;">
                        <h3 class="text-2xl font-bold text-center mb-8 ghn-orange">NGÀY 2: VÒNG ĐỐI ĐẦU & CHUNG KẾT - "BẢN LĨNH LÃNH ĐẠO"</h3>
                        <div class="relative pl-12">
                            <div class="timeline-item pb-10">
                                 <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-comments"></i></div>
                                <p class="text-sm text-slate-500">09:00 - 12:00</p>
                                <h4 class="font-bold text-lg mt-1">Vòng Đối Đầu: Tranh Biện</h4>
                                <p class="text-slate-600 text-sm mt-1">10 ứng viên (chia 2 bảng X, Y) đối đầu 1:1, tranh biện về các chủ đề quản trị hóc búa.</p>
                            </div>
                            <div class="timeline-item pb-10">
                                <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-user-tie"></i></div>
                                <p class="text-sm text-slate-500">13:30 - 15:45</p>
                                <h4 class="font-bold text-lg mt-1">Vòng Chung Kết: Đối Mặt</h4>
                                <p class="text-slate-600 text-sm mt-1">Top 4 đối mặt trực tiếp với toàn bộ HĐĐG, trả lời các câu hỏi về tầm nhìn và chiến lược.</p>
                            </div>
                            <div class="timeline-item">
                                <div class="timeline-dot bg-white border-4 border-ghn-orange flex items-center justify-center font-bold"><i class="fa-solid fa-crown"></i></div>
                                <p class="text-sm text-slate-500">16:15 - 17:00</p>
                                <h4 class="font-bold text-lg mt-1">Lễ Vinh Danh</h4>
                                <p class="text-slate-600 text-sm mt-1">Công bố người chiến thắng cuối cùng, chính thức trở thành "Chiến Tướng GHN".</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="challenge" class="py-24 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Thử Thách Lãnh Đạo Chiến Lược (12 giờ)</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Bài đánh giá cá nhân cường độ cao, yêu cầu ứng viên giải quyết hai kịch bản chiến lược phức tạp trong 12 giờ để kiểm tra toàn diện năng lực lãnh đạo.</p>
                </div>
                <div class="grid md:grid-cols-2 gap-8 max-w-6xl mx-auto">
                    <div class="bg-slate-50 p-8 rounded-xl border border-slate-200 fade-in">
                        <div class="flex items-center text-ghn-orange mb-4">
                            <i class="fa-solid fa-map-location-dot text-3xl mr-4"></i>
                            <h3 class="text-2xl font-bold text-slate-900">Kịch bản 1: Chinh Phục Thị Trường Nội Tại</h3>
                        </div>
                        <p class="text-slate-600 mb-4"><strong>Mục tiêu:</strong> Đánh giá mức độ am hiểu nội tại, khả năng phân tích dữ liệu và xây dựng chiến lược vận hành hiệu quả.</p>
                        <p class="text-slate-600"><strong>Nhiệm vụ:</strong> Phân tích một thách thức vận hành cụ thể (suy giảm thị phần, chi phí tăng...), xác định nguyên nhân gốc rễ (SWOT, 7S) và đề xuất kế hoạch hành động chi tiết.</p>
                    </div>
                     <div class="bg-slate-50 p-8 rounded-xl border border-slate-200 fade-in" style="animation-delay: 0.2s;">
                        <div class="flex items-center text-ghn-orange mb-4">
                            <i class="fa-solid fa-lightbulb text-3xl mr-4"></i>
                            <h3 class="text-2xl font-bold text-slate-900">Kịch bản 2: Kiến Tạo Thị Trường Tương Lai</h3>
                        </div>
                        <p class="text-slate-600 mb-4"><strong>Mục tiêu:</strong> Đánh giá tầm nhìn chiến lược, khả năng thích ứng và năng lực dẫn dắt trong môi trường bất định.</p>
                        <p class="text-slate-600"><strong>Nhiệm vụ:</strong> Phân tích tác động của một biến động giả định (công nghệ mới, chính sách thay đổi...), phát triển các kịch bản chiến lược (PESTEL, 5 Forces) và trình bày một tầm nhìn dài hạn.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="criteria" class="py-24">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Hệ Thống Đánh Giá Minh Bạch & Công Bằng</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Chúng tôi áp dụng một hệ thống đánh giá khách quan với các tiêu chí và trọng số rõ ràng, đảm bảo mọi quyết định đều dựa trên năng lực thực sự.</p>
                </div>
                <div class="max-w-4xl mx-auto" x-data="{ openAccordion: 1 }">
                    <div class="bg-white rounded-xl shadow-lg p-6 sm:p-8">
                        <div class="accordion-item mb-4 border-b pb-4 fade-in">
                            <button @click="openAccordion = (openAccordion === 1 ? null : 1)" class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-slate-800">Khung Tỷ Trọng Đánh Giá Tổng Thể</h3>
                                <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300" :class="{ 'rotate-45': openAccordion === 1 }"><i class="fa-solid fa-plus"></i></span>
                            </button>
                            <div class="accordion-content" x-show="openAccordion === 1" x-transition:enter="transition ease-out duration-300" x-transition:enter-start="opacity-0 -translate-y-2" x-transition:enter-end="opacity-100 translate-y-0" x-transition:leave="transition ease-in duration-200" x-transition:leave-start="opacity-100 translate-y-0" x-transition:leave-end="opacity-0 -translate-y-2">
                                <p class="text-slate-600 mt-4 mb-4">Tổng điểm được tính toán dựa trên trọng số của ba module đánh giá chính, đảm bảo sự cân bằng giữa các hình thức thể hiện năng lực khác nhau.</p>
                                <div class="chart-container h-64 md:h-80 mx-auto"><canvas id="weightingChart"></canvas></div>
                            </div>
                        </div>
                        <div class="accordion-item mb-4 border-b pb-4 fade-in">
                             <button @click="openAccordion = (openAccordion === 2 ? null : 2)" class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-slate-800">Thang Điểm Đánh Giá Năng Lực (5 mức)</h3>
                                <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300" :class="{ 'rotate-45': openAccordion === 2 }"><i class="fa-solid fa-plus"></i></span>
                            </button>
                             <div class="accordion-content" x-show="openAccordion === 2" x-transition>
                                <p class="text-slate-600 mt-4 mb-4">Mỗi năng lực được đánh giá trên thang điểm 5, với mô tả hành vi rõ ràng cho từng cấp độ, đảm bảo tính nhất quán và khách quan.</p>
                                <ul class="space-y-3 text-slate-700">
                                    <li><strong class="text-slate-900">5 - Hình Mẫu (Role Model):</strong> Thể hiện năng lực xuất sắc, là hình mẫu cho người khác.</li>
                                    <li><strong class="text-slate-900">4 - Vượt Kỳ Vọng (Exceeds Expectations):</strong> Thể hiện năng lực hiệu quả trong hầu hết các tình huống.</li>
                                    <li><strong class="text-slate-900">3 - Đạt Yêu Cầu (Meets Expectations):</strong> Đáp ứng các yêu cầu cơ bản của năng lực.</li>
                                    <li><strong class="text-slate-900">2 - Cần Cải Thiện (Needs Development):</strong> Thể hiện năng lực không nhất quán, còn hạn chế.</li>
                                    <li><strong class="text-slate-900">1 - Yếu Kém (Unsatisfactory):</strong> Không thể hiện được các hành vi cần thiết.</li>
                                </ul>
                            </div>
                        </div>
                        <div class="accordion-item fade-in">
                             <button @click="openAccordion = (openAccordion === 3 ? null : 3)" class="accordion-header w-full text-left flex justify-between items-center">
                                <h3 class="text-xl font-semibold text-slate-800">Quy Trình Hiệu Chỉnh & Đào Tạo</h3>
                                 <span class="accordion-icon text-2xl ghn-orange transition-transform duration-300" :class="{ 'rotate-45': openAccordion === 3 }"><i class="fa-solid fa-plus"></i></span>
                            </button>
                             <div class="accordion-content" x-show="openAccordion === 3" x-transition>
                                <p class="text-slate-600 mt-4">Để đảm bảo tính công bằng, GHN áp dụng quy trình hiệu chỉnh (calibration) kết quả, nơi các thành viên Hội đồng Đánh giá cùng thảo luận để đi đến đồng thuận. Tất cả người đánh giá đều được đào tạo kỹ lưỡng về khung năng lực và kỹ thuật đánh giá khách quan.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="roadmap" class="py-24 bg-white">
             <div class="container mx-auto px-6">
                <div class="text-center mb-16 fade-in">
                    <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Lộ Trình Phát Triển Năng Lực Thực Thi</h2>
                    <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Hành trình không kết thúc sau cuộc thi. Các "Chiến Tướng" được lựa chọn sẽ bước vào một lộ trình phát triển và theo dõi năng lực thực thi chiến lược bài bản.</p>
                </div>
                <div class="grid md:grid-cols-3 gap-8">
                    <div class="text-center p-6 fade-in">
                        <div class="bg-ghn-orange text-white rounded-full w-20 h-20 flex items-center justify-center mx-auto text-3xl font-bold mb-4">3M</div>
                        <h3 class="font-bold text-xl mb-2">Giai đoạn 3 Tháng</h3>
                        <p class="text-slate-600">Hoàn thiện kế hoạch chi tiết, huy động đội ngũ và nguồn lực, đạt được những "thành công nhanh" (quick wins) để tạo đà.</p>
                    </div>
                    <div class="text-center p-6 fade-in" style="animation-delay: 0.2s;">
                        <div class="bg-slate-800 text-white rounded-full w-20 h-20 flex items-center justify-center mx-auto text-3xl font-bold mb-4">6M</div>
                        <h3 class="font-bold text-xl mb-2">Giai đoạn 6 Tháng</h3>
                        <p class="text-slate-600">Duy trì tiến độ, quản lý các thách thức phức tạp, giám sát hiệu suất và thực hiện các điều chỉnh cần thiết.</p>
                    </div>
                     <div class="text-center p-6 fade-in" style="animation-delay: 0.4s;">
                        <div class="bg-slate-500 text-white rounded-full w-20 h-20 flex items-center justify-center mx-auto text-3xl font-bold mb-4">12M</div>
                        <h3 class="font-bold text-xl mb-2">Giai đoạn 12 Tháng</h3>
                        <p class="text-slate-600">Hoàn thành mục tiêu chính, đánh giá tác động toàn diện, rút ra bài học kinh nghiệm và đảm bảo tính bền vững của kết quả.</p>
                    </div>
                </div>
            </div>
        </section>

    </main>
    
    <footer class="bg-slate-900 text-white">
        <div class="container mx-auto px-6 py-12 text-center">
            <p>&copy; 2025 Giao Hàng Nhanh. All Rights Reserved.</p>
            <p class="text-sm text-slate-400 mt-2">Hành Trình Kiến Tạo & Phát Triển Lãnh Đạo Tương Lai</p>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js" defer></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const menuBtn = document.getElementById('menu-btn');
            const mobileMenu = document.getElementById('mobile-menu');
            
            menuBtn.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });

            document.querySelectorAll('#mobile-menu a').forEach(link => {
                link.addEventListener('click', () => {
                    mobileMenu.classList.add('hidden');
                });
            });

            const sections = document.querySelectorAll('main section');
            const headerNavLinks = document.querySelectorAll('header nav a');
            
            const observerOptions = {
                root: null,
                rootMargin: '-40% 0px -60% 0px',
                threshold: 0
            };

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        const id = entry.target.getAttribute('id');
                        headerNavLinks.forEach(link => {
                            link.classList.remove('active');
                            if (link.getAttribute('href') === `#${id}`) {
                                link.classList.add('active');
                            }
                        });
                    }
                });
            }, observerOptions);

            sections.forEach(section => {
                observer.observe(section);
            });

            const radarChartOptions = {
                maintainAspectRatio: false,
                scales: {
                    r: {
                        angleLines: { display: true, color: 'rgba(0, 0, 0, 0.05)' },
                        suggestedMin: 0,
                        suggestedMax: 5,
                        pointLabels: {
                            font: { size: 10, weight: '600' },
                            color: '#4b5563'
                        },
                        grid: { color: 'rgba(0, 0, 0, 0.08)' },
                        ticks: { display: false, stepSize: 1 }
                    }
                },
                plugins: {
                    legend: { display: false },
                    tooltip: {
                        callbacks: {
                            label: (context) => `${context.label.split(' ').join('\n')}: ${context.raw}`
                        }
                    }
                }
            };
            
            const competencyData = {
                strategic: { labels: ['Phân tích Thị trường', 'Hoạch định Chiến lược', 'Tư duy Đổi mới'], data: [4, 5, 4] },
                leadership: { labels: ['Truyền cảm hứng', 'Phát triển Đội ngũ', 'Quản trị Thay đổi'], data: [5, 4, 4] },
                execution: { labels: ['Lập kế hoạch', 'Ra quyết định', 'Thúc đẩy Kết quả'], data: [5, 5, 4] },
                self: { labels: ['Tự nhận thức', 'Tư duy Phát triển', 'Học hỏi Linh hoạt'], data: [4, 5, 5] }
            };
            
            function createRadarChart(canvasId, data, label) {
                new Chart(document.getElementById(canvasId), {
                    type: 'radar',
                    data: {
                        labels: data.labels,
                        datasets: [{
                            label: label, data: data.data,
                            backgroundColor: 'rgba(232, 77, 26, 0.2)',
                            borderColor: '#E84D1A',
                            borderWidth: 2,
                            pointBackgroundColor: '#E84D1A',
                            pointBorderColor: '#fff',
                            pointHoverBackgroundColor: '#fff',
                            pointHoverBorderColor: '#E84D1A'
                        }]
                    },
                    options: radarChartOptions
                });
            }
            
            if(document.getElementById('chart1')) createRadarChart('chart1', competencyData.strategic, 'Tầm Nhìn');
            if(document.getElementById('chart2')) createRadarChart('chart2', competencyData.leadership, 'Lãnh Đạo');
            if(document.getElementById('chart3')) createRadarChart('chart3', competencyData.execution, 'Thực Thi');
            if(document.getElementById('chart4')) createRadarChart('chart4', competencyData.self, 'Bản Thân');
            
            if(document.getElementById('weightingChart')){
                new Chart(document.getElementById('weightingChart'), {
                    type: 'doughnut',
                    data: {
                        labels: ['Hoạt động Nhóm & Phỏng vấn (45%)', 'Bài tập Tình huống (40%)', 'Tự đánh giá (15%)'],
                        datasets: [{
                            data: [45, 40, 15],
                            backgroundColor: ['#E84D1A', '#1f2937', '#6b7280'],
                            borderColor: '#fff',
                            borderWidth: 4,
                            hoverOffset: 8
                        }]
                    },
                    options: {
                        maintainAspectRatio: false,
                        responsive: true,
                        plugins: {
                            legend: { position: 'bottom', labels: { font: { size: 12 }, boxWidth: 15, padding: 20 } },
                            tooltip: {
                                callbacks: {
                                    label: (context) => `${context.label}: ${context.raw}%`
                                }
                            }
                        },
                        cutout: '65%'
                    }
                });
            }

            const fadeInElements = document.querySelectorAll('.fade-in');
            const fadeInObserver = new IntersectionObserver((entries, observer) => {
                entries.forEach(entry => {
                    if(entry.isIntersecting){
                        entry.target.style.animationDelay = entry.target.style.animationDelay || '0s';
                        entry.target.classList.add('visible');
                        observer.unobserve(entry.target);
                    }
                });
            }, { threshold: 0.1 });
            fadeInElements.forEach(el => fadeInObserver.observe(el));

        });
    </script>
</body>
</html>
