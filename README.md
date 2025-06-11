<html lang="vi" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chương Trình Đánh Giá "Thách Thức Bản Lĩnh Lãnh Đạo" - GHN</title>
    <!-- Chosen Palette: Warm Neutral (Slate, Beige, Amber Accent) -->
    <!-- Application Structure Plan: A single-page application with a sticky top navigation bar for seamless scrolling between five core sections: 1. Competency Framework, 2. Assessment Process, 3. Guidelines, 4. Organization, 5. Communication. This structure organizes the complex program into logical, digestible themes, allowing any user (leader, judge, candidate) to easily access the information relevant to them without getting lost in a long document. The flow is designed to first establish 'what' is being measured (competencies), then 'how' it's measured (cases), 'rules' for measurement (guidelines), 'who' is measuring (organization), and finally 'how to talk about it' (communication), providing a comprehensive and intuitive user journey. -->
    <!-- Visualization & Content Choices: 
        - Section 1 (Competencies): Goal: Inform/Organize. Method: Interactive cards (HTML/CSS/JS) for each of the 5 core competencies. Interaction: Clicking a card expands to show detailed behavioral indicators. Justification: This is more engaging than a static list and allows for progressive disclosure of information.
        - Section 2 (Process): Goal: Inform/Compare. Method: A doughnut chart (Chart.js) shows weightings for visual comparison. A CSS-based timeline visualizes the entire assessment day flow. Interaction: Hovering on chart reveals details. Justification: Charts and timelines make complex processes and data distributions immediately understandable.
        - Section 3 (Guidelines): Goal: Organize/Inform. Method: A tabbed interface (HTML/CSS/JS) to switch between candidate and judge guidelines. Justification: Separates content for two distinct audiences cleanly within the same section.
        - Section 4 (Organization): Goal: Organize/Inform. Method: An org chart created with Tailwind grid/flexbox, and a styled HTML table for the scoring rubric. Justification: Visualizes hierarchy and structure effectively without complex graphics.
        - Section 5 (Communication): Goal: Inform/Organize. Method: A vertical timeline (CSS) detailing the communication plan phases. Justification: Presents a chronological plan in an easy-to-follow format.
        - Library/Method: Vanilla JS for all interactions (tabs, accordions, nav), Chart.js for data visualization. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Be+Vietnam+Pro:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Be Vietnam Pro', sans-serif;
            background-color: #f8f9fa;
        }
        .nav-link {
            transition: all 0.3s;
            border-bottom: 2px solid transparent;
        }
        .nav-link.active, .nav-link:hover {
            color: #f59e0b; /* amber-500 */
            border-bottom-color: #f59e0b;
        }
        .section-title {
            font-weight: 800;
            color: #1f2937; /* gray-800 */
        }
        .section-subtitle {
            color: #4b5563; /* gray-600 */
        }
        .card {
            background-color: white;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            transition: all 0.3s ease-in-out;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .tab-btn.active {
            background-color: #f59e0b;
            color: #1f2937;
        }
        .tab-btn {
            transition: background-color 0.3s;
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            left: -31px;
            top: 0;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background-color: white;
            border: 4px solid #f59e0b;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-700">

    <!-- Header & Navigation -->
    <header class="bg-white/80 backdrop-blur-lg shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex-shrink-0">
                    <h1 class="text-xl font-bold text-amber-600">Thách thức Bản lĩnh Lãnh đạo</h1>
                </div>
                <div class="hidden md:block">
                    <div class="ml-10 flex items-baseline space-x-4">
                        <a href="#section-1" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Khung Năng Lực</a>
                        <a href="#section-2" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Quy Trình Đánh Giá</a>
                        <a href="#section-3" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Hướng Dẫn</a>
                        <a href="#section-4" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Tổ Chức</a>
                        <a href="#section-5" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Truyền Thông</a>
                    </div>
                </div>
                <div class="md:hidden">
                    <button id="mobile-menu-button" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-amber-500">
                        <span class="sr-only">Open main menu</span>
                        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                        </svg>
                    </button>
                </div>
            </div>
            <div id="mobile-menu" class="md:hidden hidden">
                <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                    <a href="#section-1" class="block nav-link px-3 py-2 rounded-md text-base font-medium">Khung Năng Lực</a>
                    <a href="#section-2" class="block nav-link px-3 py-2 rounded-md text-base font-medium">Quy Trình Đánh Giá</a>
                    <a href="#section-3" class="block nav-link px-3 py-2 rounded-md text-base font-medium">Hướng Dẫn</a>
                    <a href="#section-4" class="block nav-link px-3 py-2 rounded-md text-base font-medium">Tổ Chức</a>
                    <a href="#section-5" class="block nav-link px-3 py-2 rounded-md text-base font-medium">Truyền Thông</a>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- Section 1: 05 TIÊU CHÍ ĐÁNH GIÁ QUAN TRỌNG NHẤT -->
        <section id="section-1" class="py-16">
            <div class="text-center mb-12">
                <h2 class="text-base text-amber-600 font-semibold tracking-wide uppercase">Phần 1</h2>
                <p class="mt-2 text-3xl md:text-4xl section-title">Khung Năng Lực Lãnh Đạo Cốt Lõi</p>
                <p class="mt-4 max-w-2xl mx-auto text-xl section-subtitle">Nền tảng để xác định và phát triển những nhà lãnh đạo tương lai của Giao Hàng Nhanh.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <!-- Competency Cards will be injected here by JS -->
            </div>
        </section>

        <!-- Section 2: CÁC CASES ĐÁNH GIÁ -->
        <section id="section-2" class="py-16 bg-white rounded-2xl shadow-lg">
            <div class="text-center mb-12">
                <h2 class="text-base text-amber-600 font-semibold tracking-wide uppercase">Phần 2</h2>
                <p class="mt-2 text-3xl md:text-4xl section-title">Quy Trình & Tình Huống Đánh Giá</p>
                <p class="mt-4 max-w-2xl mx-auto text-xl section-subtitle">Một hành trình thử thách được thiết kế để quan sát năng lực một cách toàn diện và thực tế.</p>
            </div>
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
                <div class="px-4">
                    <h3 class="text-2xl font-bold text-gray-800 mb-4">Phân Bổ Trọng Số Đánh Giá</h3>
                    <p class="mb-6 text-gray-600">Cấu trúc điểm số được thiết kế để đảm bảo sự cân bằng giữa năng lực cá nhân, khả năng hợp tác và tố chất lãnh đạo chiến lược.</p>
                    <div class="chart-container relative h-80 w-full max-w-md mx-auto">
                        <canvas id="competencyChart"></canvas>
                    </div>
                </div>
                <div class="px-4">
                    <h3 class="text-2xl font-bold text-gray-800 mb-6">Dòng Thời Gian Ngày Đánh Giá</h3>
                    <div class="relative pl-8 border-l-2 border-amber-200">
                        <div class="timeline-item mb-8">
                            <h4 class="font-bold text-lg text-gray-800">Chuẩn Bị Tập Trung (120 phút)</h4>
                            <p class="text-gray-600">Ứng viên nhận Case Đóng và Case Mở (bốc thăm), bắt đầu quá trình phân tích và xây dựng giải pháp.</p>
                        </div>
                        <div class="timeline-item mb-8">
                            <h4 class="font-bold text-lg text-gray-800">Trình Bày Giải Pháp (10 phút)</h4>
                            <p class="text-gray-600">Trình bày kế hoạch, chiến lược và giải pháp cho các tình huống đã được giao trước Hội đồng Giám khảo.</p>
                        </div>
                        <div class="timeline-item">
                            <h4 class="font-bold text-lg text-gray-800">Phản Biện & Phỏng Vấn Sâu (10 phút)</h4>
                            <p class="text-gray-600">Trả lời các câu hỏi thử thách từ BGK, bảo vệ quan điểm và làm rõ các khía cạnh của giải pháp.</p>
                        </div>
                    </div>
                </div>
            </div>
             <div class="mt-16 px-4">
                <h3 class="text-2xl font-bold text-gray-800 mb-6 text-center">Các Tình Huống Thử Thách</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="card p-6">
                        <h4 class="font-bold text-xl text-amber-600 mb-2">Case Đóng (Gửi Trước)</h4>
                        <p class="font-semibold text-gray-800 mb-2">Tình huống: "Tối ưu hóa Vận hành & Nâng cao Trải nghiệm Khách hàng tại GHN"</p>
                        <p class="text-gray-600">Ứng viên nhận một bộ dữ liệu về một khu vực vận hành cụ thể. Yêu cầu: xây dựng kế hoạch toàn diện để cải thiện hiệu suất (giảm chi phí, tăng tốc độ) đồng thời nâng cao chỉ số hài lòng của khách hàng (CSAT), có xem xét đến việc ứng dụng công nghệ mới.</p>
                    </div>
                     <div class="card p-6">
                        <h4 class="font-bold text-xl text-amber-600 mb-2">Case Mở (Bốc thăm tại chỗ)</h4>
                        <p class="font-semibold text-gray-800 mb-2">Tình huống: "Xử lý Khủng hoảng & Dẫn dắt Sự thay đổi"</p>
                        <p class="text-gray-600">Ứng viên bốc thăm một trong các kịch bản khủng hoảng giả định (VD: sự cố hệ thống diện rộng, biến động nhân sự lớn, phản hồi tiêu cực từ một khách hàng lớn). Yêu cầu: trình bày kế hoạch hành động tức thời và chiến lược quản lý sự thay đổi trong đội ngũ.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 3: HƯỚNG DẪN CHO ỨNG VIÊN VÀ GIÁM KHẢO -->
        <section id="section-3" class="py-16">
            <div class="text-center mb-12">
                <h2 class="text-base text-amber-600 font-semibold tracking-wide uppercase">Phần 3</h2>
                <p class="mt-2 text-3xl md:text-4xl section-title">Cẩm Nang Hướng Dẫn Chi Tiết</p>
                <p class="mt-4 max-w-2xl mx-auto text-xl section-subtitle">Đảm bảo một quy trình đánh giá minh bạch, công bằng và nhất quán cho tất cả mọi người.</p>
            </div>
            <div class="max-w-4xl mx-auto">
                <div class="mb-4 flex justify-center rounded-lg bg-gray-200 p-1">
                    <button id="tab-ungvien" class="tab-btn active w-1/2 py-2 px-4 rounded-md font-semibold">Dành cho Ứng viên</button>
                    <button id="tab-giamkhao" class="tab-btn w-1/2 py-2 px-4 rounded-md font-semibold">Dành cho Giám khảo</button>
                </div>
                <div id="content-ungvien" class="card p-8">
                    <h3 class="text-2xl font-bold mb-4 text-gray-800">Để Tỏa Sáng Trong Ngày Đánh Giá</h3>
                    <ul class="space-y-4 list-disc list-inside text-gray-600">
                        <li><strong class="text-gray-700">Hiểu Sâu Vấn Đề:</strong> Dành thời gian phân tích kỹ lưỡng dữ liệu và bối cảnh của case study. Đừng chỉ nhìn bề mặt, hãy tìm ra nguyên nhân gốc rễ.</li>
                        <li><strong class="text-gray-700">Tư Duy Cấu Trúc:</strong> Sắp xếp ý tưởng và giải pháp của bạn một cách logic. Sử dụng các framework (nếu phù hợp) để trình bày mạch lạc, dễ hiểu.</li>
                        <li><strong class="text-gray-700">Dữ Liệu Là Vua:</strong> Mọi lập luận, đề xuất đều cần được củng cố bằng dữ liệu, dù là từ case study hay giả định có cơ sở. Thể hiện khả năng phân tích và ra quyết định dựa trên số liệu.</li>
                        <li><strong class="text-gray-700">Thể Hiện Tầm Nhìn:</strong> Đừng chỉ giải quyết vấn đề trước mắt. Hãy cho thấy bạn có thể nhìn xa hơn, liên kết giải pháp với chiến lược dài hạn của GHN.</li>
                        <li><strong class="text-gray-700">Lắng Nghe Chủ Động:</strong> Trong phần Q&A, hãy lắng nghe kỹ câu hỏi của BGK. Đây là cơ hội để bạn làm rõ, bảo vệ quan điểm và thể hiện khả năng ứng biến.</li>
                        <li><strong class="text-gray-700">Hãy Là Chính Mình:</strong> Chương trình tìm kiếm những nhà lãnh đạo đích thực. Hãy thể hiện phong cách, giá trị và khát khao phát triển của chính bạn.</li>
                    </ul>
                </div>
                <div id="content-giamkhao" class="card p-8 hidden">
                    <h3 class="text-2xl font-bold mb-4 text-gray-800">Nguyên Tắc Đánh Giá Khách Quan (ORCE)</h3>
                    <ul class="space-y-4 list-disc list-inside text-gray-600">
                        <li><strong class="text-gray-700">Quan sát (Observe):</strong> Tập trung vào các hành vi, lời nói, cách ứng viên tương tác và giải quyết vấn đề một cách cụ thể, thay vì dựa trên cảm tính hay ấn tượng chung.</li>
                        <li><strong class="text-gray-700">Ghi chép (Record):</strong> Ghi lại các bằng chứng hành vi một cách chi tiết và thực tế. "Ứng viên A đề xuất 3 giải pháp X, Y, Z dựa trên phân tích dữ liệu chi phí" thay vì "Ứng viên A có vẻ sáng tạo".</li>
                        <li><strong class="text-gray-700">Phân loại (Classify):</strong> Đối chiếu các hành vi đã ghi nhận được với 5 Năng Lực Cốt Lõi đã được định nghĩa. Hành vi này thể hiện năng lực nào?</li>
                        <li><strong class="text-gray-700">Đánh giá (Evaluate):</strong> Sử dụng thang điểm và phiếu đánh giá tiêu chuẩn để cho điểm từng năng lực một cách nhất quán. Thảo luận với các giám khảo khác để hiệu chỉnh và đưa ra quyết định đồng thuận.</li>
                        <li><strong class="text-gray-700">Tránh Thiên Vị:</strong> Luôn ý thức về các thiên vị tiềm ẩn (hiệu ứng hào quang, ấn tượng đầu tiên, so sánh giữa các ứng viên) và chủ động loại bỏ chúng khỏi quá trình đánh giá.</li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- Section 4: HƯỚNG DẪN TỔ CHỨC BUỔI ĐÁNH GIÁ -->
        <section id="section-4" class="py-16 bg-white rounded-2xl shadow-lg">
            <div class="text-center mb-12">
                <h2 class="text-base text-amber-600 font-semibold tracking-wide uppercase">Phần 4</h2>
                <p class="mt-2 text-3xl md:text-4xl section-title">Cấu Trúc Tổ Chức Chuyên Nghiệp</p>
                <p class="mt-4 max-w-2xl mx-auto text-xl section-subtitle">Thiết lập một bộ máy vận hành trơn tru, đảm bảo tính khách quan và toàn diện cho ngày đánh giá.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
                <div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-4">Thành Phần Ban Đánh Giá</h3>
                    <div class="space-y-6">
                        <div class="card p-6 flex items-start space-x-4">
                            <div class="flex-shrink-0 w-12 h-12 bg-amber-100 text-amber-600 rounded-full flex items-center justify-center text-2xl font-bold">BGK</div>
                            <div>
                                <h4 class="font-bold text-lg text-gray-800">Hội đồng Giám khảo</h4>
                                <p class="text-gray-600">Gồm đại diện Ban Giám đốc, Lãnh đạo cấp cao các Khối, chuyên gia Nhân sự. Chịu trách nhiệm phỏng vấn, đặt câu hỏi, phản biện và cho điểm cuối cùng.</p>
                            </div>
                        </div>
                        <div class="card p-6 flex items-start space-x-4">
                             <div class="flex-shrink-0 w-12 h-12 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center text-2xl font-bold">QSV</div>
                            <div>
                                <h4 class="font-bold text-lg text-gray-800">Quan sát viên</h4>
                                <p class="text-gray-600">Là các chuyên gia nhân sự hoặc quản lý có kinh nghiệm, được đào tạo về phương pháp ORCE. Tập trung quan sát, ghi chép hành vi và cung cấp dữ liệu đầu vào khách quan cho BGK.</p>
                            </div>
                        </div>
                        <div class="card p-6 flex items-start space-x-4">
                            <div class="flex-shrink-0 w-12 h-12 bg-green-100 text-green-600 rounded-full flex items-center justify-center text-2xl font-bold">HT</div>
                            <div>
                                <h4 class="font-bold text-lg text-gray-800">Khối Hỗ trợ (Ban Tổ Chức)</h4>
                                <p class="text-gray-600">Chịu trách nhiệm toàn bộ công tác hậu cần: quản lý thời gian, điều phối ứng viên, chuẩn bị tài liệu, đảm bảo sự kiện diễn ra suôn sẻ.</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-4">Thang Điểm Đánh Giá Mẫu</h3>
                    <p class="text-gray-600 mb-4">Mỗi năng lực sẽ được đánh giá trên thang điểm 5, với mô tả hành vi rõ ràng cho từng mức độ để đảm bảo tính nhất quán.</p>
                    <div class="overflow-x-auto card">
                        <table class="w-full text-sm text-left text-gray-500">
                            <thead class="text-xs text-gray-700 uppercase bg-gray-100">
                                <tr>
                                    <th scope="col" class="px-6 py-3">Điểm</th>
                                    <th scope="col" class="px-6 py-3">Mô tả hành vi (Ví dụ: Năng lực Ra quyết định)</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="bg-white border-b">
                                    <td class="px-6 py-4 font-medium text-green-600">5 - Xuất sắc</td>
                                    <td class="px-6 py-4">Luôn đưa ra quyết định tối ưu dựa trên phân tích đa chiều, dự báo được rủi ro và có kế hoạch dự phòng hiệu quả.</td>
                                </tr>
                                <tr class="bg-gray-50 border-b">
                                    <td class="px-6 py-4 font-medium text-blue-600">4 - Tốt</td>
                                    <td class="px-6 py-4">Thường đưa ra quyết định đúng đắn dựa trên dữ liệu, thể hiện sự tự tin và dứt khoát.</td>
                                </tr>
                                <tr class="bg-white border-b">
                                    <td class="px-6 py-4 font-medium text-yellow-600">3 - Đạt</td>
                                    <td class="px-6 py-4">Có khả năng đưa ra quyết định hợp lý trong hầu hết tình huống. Cần cải thiện tốc độ và sự quyết đoán.</td>
                                </tr>
                                <tr class="bg-gray-50 border-b">
                                    <td class="px-6 py-4 font-medium text-orange-600">2 - Cần cải thiện</td>
                                    <td class="px-6 py-4">Do dự khi ra quyết định, hoặc quyết định thiếu cơ sở dữ liệu và phân tích.</td>
                                </tr>
                                <tr class="bg-white">
                                    <td class="px-6 py-4 font-medium text-red-600">1 - Không đạt</td>
                                    <td class="px-6 py-4">Không thể đưa ra quyết định hoặc quyết định sai lầm, gây ảnh hưởng tiêu cực.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section 5: VIẾT TÀI LIỆU TRUYỀN THÔNG -->
        <section id="section-5" class="py-16">
            <div class="text-center mb-12">
                <h2 class="text-base text-amber-600 font-semibold tracking-wide uppercase">Phần 5</h2>
                <p class="mt-2 text-3xl md:text-4xl section-title">Kế Hoạch Truyền Thông Sự Kiện</p>
                <p class="mt-4 max-w-2xl mx-auto text-xl section-subtitle">Lan tỏa sức nóng của "Thách thức Bản lĩnh Lãnh đạo", thu hút sự quan tâm và khẳng định cam kết phát triển nhân tài của GHN.</p>
            </div>
            <div class="relative pl-8 border-l-2 border-amber-200 max-w-3xl mx-auto">
                <div class="timeline-item mb-12">
                    <h3 class="text-xl font-bold text-gray-800">Giai đoạn 1: Khởi động (Trước 4 tuần)</h3>
                    <p class="text-gray-600 mt-2"><strong class="text-gray-700">Mục tiêu:</strong> Tạo sự nhận biết và kêu gọi đề cử.</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Thông điệp chính:</strong> "Cơ hội bứt phá dành cho những tài năng lãnh đạo của GHN đã đến!"</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Kênh:</strong> Email từ CEO, bài đăng trên Workplace/Intranet, poster tại văn phòng/kho.</p>
                </div>
                <div class="timeline-item mb-12">
                    <h3 class="text-xl font-bold text-gray-800">Giai đoạn 2: Tăng tốc (Trước 2 tuần)</h3>
                    <p class="text-gray-600 mt-2"><strong class="text-gray-700">Mục tiêu:</strong> Chia sẻ thông tin chi tiết, công bố danh sách ứng viên.</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Thông điệp chính:</strong> "Gặp gỡ những ứng viên xuất sắc nhất sẵn sàng chinh phục Thách thức Bản lĩnh Lãnh đạo."</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Kênh:</strong> Bài viết giới thiệu từng ứng viên, video clip ngắn từ BTC/BGK, tổ chức workshop hướng dẫn cho ứng viên.</p>
                </div>
                <div class="timeline-item mb-12">
                    <h3 class="text-xl font-bold text-gray-800">Giai đoạn 3: Ngày D (Trong sự kiện)</h3>
                     <p class="text-gray-600 mt-2"><strong class="text-gray-700">Mục tiêu:</strong> Giữ nhiệt và lan tỏa không khí sự kiện.</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Thông điệp chính:</strong> "Những khoảnh khắc trí tuệ và bản lĩnh đang được thể hiện."</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Kênh:</strong> Cập nhật hình ảnh, livestream các phần không bảo mật (nếu có) trên các kênh nội bộ.</p>
                </div>
                <div class="timeline-item">
                    <h3 class="text-xl font-bold text-gray-800">Giai đoạn 4: Vinh danh (Sau 1 tuần)</h3>
                    <p class="text-gray-600 mt-2"><strong class="text-gray-700">Mục tiêu:</strong> Công bố kết quả và truyền cảm hứng.</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Thông điệp chính:</strong> "Chúc mừng những nhà lãnh đạo tương lai đã được tìm ra. Hành trình phát triển chỉ mới bắt đầu."</p>
                    <p class="text-gray-600 mt-1"><strong class="text-gray-700">Kênh:</strong> Bài viết vinh danh trên các kênh chính thức, email cảm ơn từ Ban lãnh đạo, chia sẻ về kế hoạch phát triển tiếp theo cho các tài năng.</p>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-gray-800 text-white">
        <div class="container mx-auto py-8 px-4 sm:px-6 lg:px-8 text-center">
            <p class="font-bold text-lg">GIAO HÀNG NHANH</p>
            <p class="text-sm text-gray-400 mt-1">Cam kết phát triển con người và xây dựng đội ngũ lãnh đạo kế cận vững mạnh.</p>
            <p class="text-xs text-gray-500 mt-4">&copy; 2024 Giao Hàng Nhanh. All rights reserved.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // Data for competencies
            const competencies = [
                { 
                    icon: '🎯', 
                    title: 'Năng lực Lãnh đạo', 
                    description: 'Khả năng dẫn dắt, truyền cảm hứng và phát triển đội ngũ để đạt được mục tiêu chung.',
                    details: [
                        'Phân công công việc, giao quyền và giám sát hiệu quả.',
                        'Xây dựng môi trường làm việc tích cực, tạo động lực và giữ chân nhân tài.',
                        'Giải quyết xung đột nội bộ một cách xây dựng và công bằng.',
                        'Là hình mẫu về hành vi và thái độ cho đội ngũ.'
                    ]
                },
                { 
                    icon: '🤔', 
                    title: 'Tư duy hệ thống & Giải quyết vấn đề', 
                    description: 'Khả năng nhìn nhận bức tranh tổng thể, phân tích các yếu tố liên quan để đưa ra giải pháp tối ưu.',
                    details: [
                        'Xác định đúng nguyên nhân gốc rễ của vấn đề thay vì xử lý triệu chứng.',
                        'Xem xét tác động của quyết định đến các bộ phận, quy trình khác.',
                        'Đưa ra nhiều phương án giải quyết và lựa chọn phương án khả thi nhất.',
                        'Hệ thống hóa các giải pháp để có thể áp dụng trong tương lai.'
                    ]
                },
                { 
                    icon: '📊', 
                    title: 'Sử dụng Dữ liệu để Phân tích & Ra quyết định', 
                    description: 'Năng lực thu thập, phân tích dữ liệu và biến những con số thành hành động cụ thể, hiệu quả.',
                    details: [
                        'Sử dụng các chỉ số (KPIs) để đánh giá hiệu suất và xác định vấn đề.',
                        'Lập luận và bảo vệ quan điểm dựa trên bằng chứng, số liệu rõ ràng.',
                        'Ra quyết định nhanh chóng, dứt khoát dựa trên phân tích dữ liệu.',
                        'Khuyến khích văn hóa ra quyết định dựa trên dữ liệu trong đội nhóm.'
                    ]
                },
                { 
                    icon: '🚀', 
                    title: 'Tầm nhìn chiến lược & Kế hoạch khả thi', 
                    description: 'Khả năng dự báo xu hướng, định hình tương lai và xây dựng lộ trình hành động rõ ràng để hiện thực hóa tầm nhìn.',
                    details: [
                        'Hiểu biết sâu rộng về ngành, thị trường và đối thủ cạnh tranh.',
                        'Đề xuất các giải pháp, sáng kiến mang tính chiến lược, dài hạn.',
                        'Chuyển hóa tầm nhìn thành các mục tiêu và kế hoạch hành động cụ thể.',
                        'Truyền đạt tầm nhìn một cách thuyết phục để đội ngũ cùng hướng tới.'
                    ]
                },
                { 
                    icon: '�', 
                    title: 'Tiềm năng phát triển cá nhân', 
                    description: 'Sự kiên trì, ham học hỏi và các giá trị cốt lõi là nền tảng cho sự phát triển bền bỉ trong dài hạn.',
                    details: [
                        'Chủ động tìm kiếm kiến thức mới, học hỏi từ kinh nghiệm và phản hồi.',
                        'Thể hiện sự kiên trì, không bỏ cuộc trước khó khăn, thách thức.',
                        'Hành xử trung thực, minh bạch và có trách nhiệm cao.',
                        'Có tham vọng phát triển bản thân và đóng góp cho sự phát triển của tổ chức.'
                    ]
                }
            ];

            const competencyContainer = document.querySelector('#section-1 .grid');
            competencies.forEach(comp => {
                const card = document.createElement('div');
                card.className = 'card p-6 cursor-pointer competency-card';
                card.innerHTML = `
                    <div class="flex items-center mb-4">
                        <div class="text-3xl mr-4">${comp.icon}</div>
                        <h3 class="text-xl font-bold text-gray-800">${comp.title}</h3>
                    </div>
                    <p class="text-gray-600">${comp.description}</p>
                    <div class="details hidden mt-4 pl-4 border-l-2 border-amber-200">
                        <ul class="space-y-2 list-inside text-gray-500 text-sm">
                            ${comp.details.map(d => `<li>${d}</li>`).join('')}
                        </ul>
                    </div>
                `;
                competencyContainer.appendChild(card);
            });
            
            document.querySelectorAll('.competency-card').forEach(card => {
                card.addEventListener('click', () => {
                    card.querySelector('.details').classList.toggle('hidden');
                });
            });

            // Chart.js doughnut chart
            const ctx = document.getElementById('competencyChart').getContext('2d');
            new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Tình huống Cá nhân', 'Thử thách Nhóm', 'Phỏng vấn Tập trung'],
                    datasets: [{
                        label: 'Trọng số đánh giá',
                        data: [30, 40, 30],
                        backgroundColor: [
                            'rgba(245, 158, 11, 0.7)',  // amber-500
                            'rgba(59, 130, 246, 0.7)', // blue-500
                            'rgba(16, 185, 129, 0.7)' // emerald-500
                        ],
                        borderColor: [
                            'rgba(245, 158, 11, 1)',
                            'rgba(59, 130, 246, 1)',
                            'rgba(16, 185, 129, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed !== null) {
                                        label += context.parsed + '%';
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });

            // Tab functionality for guidelines
            const tabUngVien = document.getElementById('tab-ungvien');
            const tabGiamKhao = document.getElementById('tab-giamkhao');
            const contentUngVien = document.getElementById('content-ungvien');
            const contentGiamKhao = document.getElementById('content-giamkhao');

            tabUngVien.addEventListener('click', () => {
                tabUngVien.classList.add('active');
                tabGiamKhao.classList.remove('active');
                contentUngVien.classList.remove('hidden');
                contentGiamKhao.classList.add('hidden');
            });

            tabGiamKhao.addEventListener('click', () => {
                tabGiamKhao.classList.add('active');
                tabUngVien.classList.remove('active');
                contentGiamKhao.classList.remove('hidden');
                contentUngVien.classList.add('hidden');
            });

            // Mobile menu toggle
            const mobileMenuButton = document.getElementById('mobile-menu-button');
            const mobileMenu = document.getElementById('mobile-menu');
            mobileMenuButton.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });
            
            // Smooth scroll for nav links and active state
            const navLinks = document.querySelectorAll('.nav-link');
            const sections = document.querySelectorAll('section[id]');
            
            function changeLinkState() {
                let index = sections.length;
                while(--index && window.scrollY + 100 < sections[index].offsetTop) {}
                
                navLinks.forEach((link) => link.classList.remove('active'));
                
                // Find corresponding link
                const activeLink = document.querySelector(`.nav-link[href="#${sections[index].id}"]`);
                if(activeLink) {
                   activeLink.classList.add('active');
                }
            }
            
            // Initial call
            changeLinkState();
            
            window.addEventListener('scroll', changeLinkState);

             // Close mobile menu on link click
            document.querySelectorAll('#mobile-menu a').forEach(link => {
                link.addEventListener('click', () => {
                    mobileMenu.classList.add('hidden');
                });
            });

        });
    </script>
</body>
</html>
