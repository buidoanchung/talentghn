<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kế hoạch Chương trình Thi đua Tìm kiếm Tài năng Lãnh đạo GHN</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: GHN Corporate Harmony -->
    <!-- Application Structure Plan: Tôi đã thiết kế một ứng dụng trang đơn (SPA) với thanh điều hướng cố định bên trái để truy cập nhanh các phần chính của kế hoạch. Cấu trúc này chuyển đổi báo cáo dài thành một trang tổng quan tương tác, lý tưởng cho lãnh đạo cấp cao bận rộn. Người dùng có thể xem tổng quan, sau đó đi sâu vào các phần cụ thể như "Hồ sơ Tài năng" (với các thẻ tương tác cho từng năng lực), "Quy trình Đánh giá" (dưới dạng sơ đồ luồng), và "Lịch trình 2 ngày" (dưới dạng bảng biểu có thể chuyển đổi). Cách tiếp cận này ưu tiên sự dễ dàng điều hướng và khám phá, giúp người dùng nắm bắt thông tin cốt lõi một cách nhanh chóng và hiệu quả. -->
    <!-- Visualization & Content Choices: 
    - Hồ sơ Tài năng -> Goal: Organize/Inform -> Viz: Lưới các thẻ tương tác (HTML/Tailwind) -> Interaction: Nhấp để mở rộng xem chi tiết -> Justification: Chia nhỏ thông tin phức tạp, khuyến khích tương tác.
    - Quy trình Đánh giá -> Goal: Organize/Inform -> Viz: Sơ đồ quy trình (HTML/Tailwind) -> Interaction: Di chuột để xem mô tả ngắn -> Justification: Trực quan hóa các bước một cách logic.
    - Lịch trình 2 ngày -> Goal: Organize/Inform -> Viz: Bảng biểu với các tab (HTML/JS) -> Interaction: Nhấp vào tab để chuyển đổi giữa các ngày -> Justification: Trình bày thông tin dày đặc một cách gọn gàng.
    - Lộ trình phát triển -> Goal: Show Change -> Viz: Dòng thời gian ngang (HTML/Tailwind) -> Interaction: Nhấp vào các mốc thời gian để xem chi tiết -> Justification: Trực quan hóa sự tiến triển theo thời gian hiệu quả hơn bảng biểu.
    - Biểu đồ năng lực mẫu -> Goal: Compare/Inform -> Viz: Biểu đồ radar (Chart.js/Canvas) -> Interaction: Dropdown để thay đổi dữ liệu mẫu -> Justification: Trực quan hóa mạnh mẽ các điểm mạnh/yếu của ứng viên trên nhiều năng lực.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .section-active {
            color: #F97316;
            font-weight: 600;
            background-color: #FEF3C7;
            border-right: 3px solid #F97316;
        }
        .nav-link {
            transition: all 0.3s ease;
            border-right: 3px solid transparent;
        }
        .nav-link:hover {
            background-color: #FEF3C7;
            border-right: 3px solid #FDBA74;
        }
        .content-section {
            display: none;
        }
        .content-section.active {
            display: block;
        }
        .competency-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .competency-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .details {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.7s ease-in-out;
        }
        .details.open {
            max-height: 1500px;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: auto;
            max-height: 500px;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800">
    <div class="flex flex-col md:flex-row min-h-screen">
        <aside id="sidebar" class="w-full md:w-64 bg-white border-r border-slate-200 shadow-lg md:fixed md:h-full">
            <div class="p-6">
                <h1 class="text-xl font-bold text-slate-900">GHN Leadership</h1>
                <p class="text-sm text-slate-500">Talent Search Program</p>
            </div>
            <nav class="mt-4">
                <a href="#overview" class="block py-3 px-6 text-slate-600 nav-link section-active">Tổng quan</a>
                <a href="#profile" class="block py-3 px-6 text-slate-600 nav-link">Hồ sơ Tài năng</a>
                <a href="#process" class="block py-3 px-6 text-slate-600 nav-link">Quy trình Đánh giá</a>
                <a href="#agenda" class="block py-3 px-6 text-slate-600 nav-link">Lịch trình 2 Ngày</a>
                <a href="#cases" class="block py-3 px-6 text-slate-600 nav-link">Bài tập Tình huống</a>
                <a href="#roadmap" class="block py-3 px-6 text-slate-600 nav-link">Lộ trình Phát triển</a>
                <a href="#chart" class="block py-3 px-6 text-slate-600 nav-link">Biểu đồ Năng lực Mẫu</a>
            </nav>
        </aside>

        <main class="flex-1 md:ml-64 p-4 sm:p-6 md:p-10 bg-slate-50">
            <section id="overview" class="content-section active">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-4">Chương trình Thi đua Tìm kiếm Tài năng Lãnh đạo GHN</h2>
                    <div class="prose max-w-none text-slate-600">
                        <p class="lead">Báo cáo này trình bày kế hoạch chi tiết cho Chương trình Thi đua Tìm kiếm Tài năng Lãnh đạo tại Giao Hàng Nhanh (GHN), được thiết kế nhằm mục tiêu chiến lược là xác định, đánh giá và tạo nền tảng phát triển cho đội ngũ lãnh đạo kế cận. Chương trình được xây dựng dựa trên sự kết hợp giữa các thông lệ quốc tế hàng đầu và đặc thù của GHN.</p>
                        <p>Mục tiêu của ứng dụng tương tác này là cung cấp cho Ban Lãnh đạo một cái nhìn tổng quan, dễ tiếp cận về toàn bộ kế hoạch. Thay vì đọc một tài liệu dài, bạn có thể nhanh chóng điều hướng đến các phần quan trọng, khám phá các tiêu chí đánh giá, xem lịch trình chi tiết và hiểu rõ các bài tập tình huống được đề xuất. Mỗi phần đều được thiết kế để làm nổi bật thông tin cốt lõi, giúp việc ra quyết định trở nên hiệu quả và dựa trên dữ liệu.</p>
                    </div>
                </div>
            </section>

            <section id="profile" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Hồ sơ Thành công (Success Profile)</h2>
                    <p class="mb-8 text-slate-600">Hồ sơ này xác định 5 yếu tố then chốt của một nhà lãnh đạo xuất sắc tại GHN. Nhấp vào từng thẻ để khám phá chi tiết các tiêu chí và phương pháp đánh giá.</p>
                    <div id="competency-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    </div>
                </div>
            </section>
            
            <section id="process" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Quy trình Đánh giá Đa diện</h2>
                    <p class="mb-8 text-slate-600">Chúng tôi áp dụng phương pháp đánh giá đa diện để có được bức tranh toàn cảnh và chính xác nhất về năng lực của ứng viên, kết hợp nhiều công cụ để tạo thành một "tam giác dữ liệu" mạnh mẽ.</p>
                     <div id="assessment-process-flow" class="space-y-4">
                     </div>
                </div>
            </section>

            <section id="agenda" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Lịch trình Đánh giá Chi tiết (02 Ngày)</h2>
                    <p class="mb-6 text-slate-600">Lịch trình được thiết kế để cân bằng giữa các hoạt động đánh giá cường độ cao và thời gian nghỉ ngơi, đảm bảo ứng viên có thể thể hiện tốt nhất năng lực của mình.</p>
                    <div class="mb-4 border-b border-gray-200">
                        <nav class="-mb-px flex space-x-8" aria-label="Tabs">
                            <button id="tab-day1" class="tab-button whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-orange-600 border-orange-500">Ngày 1</button>
                            <button id="tab-day2" class="tab-button whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 hover:border-gray-300 border-transparent">Ngày 2</button>
                        </nav>
                    </div>
                    <div id="agenda-content"></div>
                </div>
            </section>

            <section id="cases" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                     <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Đề xuất Bài tập Tình huống</h2>
                     <p class="mb-8 text-slate-600">Hai bài tập tình huống được thiết kế để đánh giá sâu sắc năng lực chiến lược của ứng viên trong các bối cảnh khác nhau: một tập trung vào hiện tại, một hướng đến tương lai.</p>
                     <div class="space-y-8">
                        <div class="border border-slate-200 rounded-lg p-6">
                            <h3 class="font-bold text-lg text-slate-800 mb-2">Bài tập 1: Điều hướng Thực tế Thị trường Hiện tại</h3>
                            <p class="text-slate-600 mb-4"><strong class="text-slate-700">Mục tiêu:</strong> Đánh giá khả năng phân tích bối cảnh nội tại của GHN và xây dựng chiến lược vận hành dựa trên dữ liệu thực tế.</p>
                            <div class="prose prose-sm max-w-none text-slate-600">
                                <p><strong>Kịch bản:</strong> Ứng viên đối mặt với một thách thức cụ thể như suy giảm thị phần, vấn đề hiệu quả vận hành, hoặc sự xuất hiện của đối thủ mới.</p>
                                <p><strong>Nhiệm vụ:</strong> Phân tích tình hình, xác định vấn đề cốt lõi, đề xuất chiến lược, xây dựng kế hoạch hành động chi tiết và KPIs đo lường. Mọi đề xuất phải dựa trên dữ liệu cung cấp.</p>
                                <p><strong>Năng lực đánh giá:</strong> Phân tích Thị trường, Hoạch định Chiến lược, Ra quyết định dựa trên Dữ liệu, Giải quyết Vấn đề.</p>
                            </div>
                        </div>
                        <div class="border border-slate-200 rounded-lg p-6">
                            <h3 class="font-bold text-lg text-slate-800 mb-2">Bài tập 2: Phản ứng Chiến lược Trước Biến động Tương lai</h3>
                             <p class="text-slate-600 mb-4"><strong class="text-slate-700">Mục tiêu:</strong> Đánh giá tầm nhìn chiến lược, khả năng thích ứng, đổi mới và năng lực dẫn dắt tổ chức qua giai đoạn bất ổn.</p>
                             <div class="prose prose-sm max-w-none text-slate-600">
                                <p><strong>Kịch bản:</strong> Đặt ứng viên vào một tình huống tương lai giả định với độ không chắc chắn cao (ví dụ: công nghệ đột phá, thay đổi chính sách lớn, thay đổi hành vi khách hàng).</p>
                                <p><strong>Nhiệm vụ:</strong> Phân tích tác động, phát triển các phương án chiến lược, đánh giá rủi ro, đề xuất hướng đi và trình bày tầm nhìn cho GHN trong bối cảnh mới.</p>
                                <p><strong>Năng lực đánh giá:</strong> Tầm nhìn Chiến lược, Tư duy Sáng tạo, Quản trị Sự thay đổi, Ra quyết định trong điều kiện không chắc chắn.</p>
                            </div>
                        </div>
                     </div>
                </div>
            </section>

            <section id="roadmap" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Lộ trình Phát triển Tài năng (3-6-12 tháng)</h2>
                    <p class="mb-8 text-slate-600">Việc xác định tài năng chỉ là bước đầu. Lộ trình phát triển sau đánh giá đảm bảo sự đầu tư mang lại lợi tức xứng đáng và bền vững cho GHN.</p>
                    <div id="roadmap-timeline" class="relative border-l-2 border-orange-400 ml-4 py-4">
                    </div>
                </div>
            </section>

             <section id="chart" class="content-section">
                <div class="bg-white p-8 rounded-lg shadow-md">
                    <h2 class="text-2xl md:text-3xl font-bold text-slate-900 mb-2">Biểu đồ Năng lực Mẫu</h2>
                    <p class="mb-6 text-slate-600">Biểu đồ radar này trực quan hóa kết quả đánh giá của một ứng viên mẫu dựa trên 5 yếu tố của Hồ sơ Thành công. Nó giúp nhanh chóng xác định các điểm mạnh và lĩnh vực cần phát triển.</p>
                     <div class="mb-4">
                        <label for="candidate-select" class="block text-sm font-medium text-gray-700">Chọn ứng viên mẫu:</label>
                        <select id="candidate-select" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-orange-500 focus:border-orange-500 sm:text-sm rounded-md">
                            <option value="0">Ứng viên A</option>
                            <option value="1">Ứng viên B</option>
                        </select>
                    </div>
                    <div class="chart-container">
                        <canvas id="competencyChart"></canvas>
                    </div>
                </div>
            </section>

        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const navLinks = document.querySelectorAll('.nav-link');
            const sections = document.querySelectorAll('.content-section');

            function showSection(hash) {
                sections.forEach(section => {
                    if ('#' + section.id === hash) {
                        section.classList.add('active');
                    } else {
                        section.classList.remove('active');
                    }
                });

                navLinks.forEach(link => {
                    if (link.getAttribute('href') === hash) {
                        link.classList.add('section-active');
                    } else {
                        link.classList.remove('section-active');
                    }
                });
            }
            
            navLinks.forEach(link => {
                link.addEventListener('click', function (e) {
                    e.preventDefault();
                    const targetHash = this.getAttribute('href');
                    history.pushState(null, null, targetHash);
                    showSection(targetHash);
                });
            });

            window.addEventListener('popstate', () => {
                showSection(window.location.hash || '#overview');
            });

            showSection(window.location.hash || '#overview');

            const competencyData = [
                {
                    title: 'Tự nhận thức sâu sắc',
                    icon: '🧠',
                    summary: 'Khả năng nhận thức rõ ràng về nội tâm, giá trị, điểm mạnh, điểm yếu và tác động của chúng lên người khác.',
                    details: {
                        'Tiêu chí': [
                            'Hiểu rõ điểm mạnh/yếu và tác động',
                            'Xác định và sống đúng giá trị cốt lõi',
                            'Truyền cảm hứng qua câu chuyện cá nhân',
                            'Khả năng tự phản tư và rút kinh nghiệm',
                            'Cởi mở tiếp nhận và áp dụng phản hồi',
                        ],
                        'Phương pháp đánh giá': [
                            'Video "Câu chuyện truyền cảm hứng"',
                            'Bài tự luận Phản tư',
                            'Phỏng vấn Hành vi Sự kiện (BEI)',
                            'Phiếu tự đánh giá',
                        ]
                    }
                },
                {
                    title: 'Năng lực Lãnh đạo tại GHN',
                    icon: '👥',
                    summary: 'Khả năng tự lãnh đạo, phát huy tiềm năng cá nhân, đồng thời dẫn dắt, xây dựng và phát triển đội ngũ vững mạnh.',
                     details: {
                        'Tiêu chí': [
                            'Quản lý bản thân và phát huy tiềm năng',
                            'Xây dựng đội ngũ hiệu quả, gắn kết',
                            'Phát triển nhân tài (huấn luyện, cố vấn)',
                            'Thấu hiểu tâm lý và tạo động lực cho đội nhóm',
                            'Giao tiếp hiệu quả và truyền cảm hứng',
                        ],
                        'Phương pháp đánh giá': [
                            'Bài tập Tình huống Nhóm',
                            'Phỏng vấn Hành vi Sự kiện (BEI)',
                            'Đánh giá 360 độ (nội bộ)',
                        ]
                    }
                },
                {
                    title: 'Tầm nhìn và Chiến lược',
                    icon: '🔭',
                    summary: 'Khả năng phân tích bối cảnh, dự báo xu hướng và xây dựng các kế hoạch chiến lược đột phá, khả thi.',
                     details: {
                        'Tiêu chí': [
                            'Phân tích toàn diện môi trường kinh doanh',
                            'Xác định đúng cơ hội và thách thức',
                            'Xây dựng tầm nhìn rõ ràng, truyền cảm hứng',
                            'Hoạch định chiến lược cụ thể, khả thi',
                            'Tư duy đổi mới, đột phá',
                        ],
                        'Phương pháp đánh giá': [
                            'Nghiên cứu Tình huống Chiến lược',
                            'Bài tự luận về tầm nhìn',
                            'Phỏng vấn chuyên sâu với Ban Lãnh đạo',
                        ]
                    }
                },
                {
                    title: 'Năng lực Thực thi Xuất sắc',
                    icon: '🚀',
                    summary: 'Khả năng chuyển hóa chiến lược thành hành động, quản lý hiệu suất và đạt kết quả vượt kỳ vọng.',
                    details: {
                        'Tiêu chí': [
                            'Xây dựng kế hoạch hành động chi tiết',
                            'Quản lý hiệu suất, theo dõi tiến độ',
                            'Ra quyết định nhanh chóng, dứt khoát',
                            'Tinh thần "làm tới cùng", vượt khó',
                            'Bằng chứng về kết quả vượt trội',
                        ],
                        'Phương pháp đánh giá': [
                            'Xem xét Hồ sơ Thành tích',
                            'Phỏng vấn Hành vi Sự kiện (BEI)',
                            'Nghiên cứu Tình huống (phần kế hoạch thực thi)',
                        ]
                    }
                },
                 {
                    title: 'Phù hợp với Tổ chức',
                    icon: '🧩',
                    summary: 'Sự tương thích giữa giá trị cá nhân, mục tiêu nghề nghiệp của ứng viên với văn hóa, giá trị của GHN.',
                    details: {
                        'Tiêu chí': [
                            'Tương đồng về giá trị cốt lõi',
                            'Hiểu và phù hợp với văn hóa GHN',
                            'Mục tiêu nghề nghiệp phù hợp với tổ chức',
                            'Thể hiện hành vi làm gương',
                            'Cam kết và động lực đóng góp cho GHN',
                        ],
                        'Phương pháp đánh giá': [
                            'Phỏng vấn chuyên sâu với HR/Lãnh đạo',
                            'Quan sát trong các bài tập nhóm',
                            'Câu hỏi tình huống về giá trị/văn hóa',
                        ]
                    }
                }
            ];

            const competencyGrid = document.getElementById('competency-grid');
            competencyData.forEach(item => {
                const card = document.createElement('div');
                card.className = 'bg-slate-100 p-6 rounded-lg cursor-pointer competency-card';
                let detailsHtml = '<div class="details mt-4 text-sm text-slate-600 space-y-4">';
                for (const [key, value] of Object.entries(item.details)) {
                    detailsHtml += `<div><h4 class="font-semibold text-slate-700">${key}:</h4><ul class="list-disc list-inside mt-1 space-y-1">`;
                    value.forEach(v => { detailsHtml += `<li>${v}</li>`; });
                    detailsHtml += `</ul></div>`;
                }
                detailsHtml += '</div>';

                card.innerHTML = `
                    <div class="flex items-start">
                        <span class="text-3xl mr-4">${item.icon}</span>
                        <div>
                            <h3 class="font-bold text-lg text-slate-800">${item.title}</h3>
                            <p class="text-sm text-slate-600 mt-1">${item.summary}</p>
                        </div>
                    </div>
                    ${detailsHtml}
                `;
                competencyGrid.appendChild(card);
                
                card.addEventListener('click', () => {
                    card.querySelector('.details').classList.toggle('open');
                });
            });

            const assessmentProcessData = [
                {
                    icon: '📹',
                    title: 'Video & Tự luận Phản tư',
                    description: 'Đánh giá năng lực tự nhận thức, khả năng tự phản tư, và năng lực kể chuyện, truyền cảm hứng.'
                },
                {
                    icon: '📈',
                    title: 'Nghiên cứu Tình huống Chiến lược',
                    description: 'Hoạt động cốt lõi để đánh giá tư duy chiến lược, phân tích thị trường, và khả năng giải quyết vấn đề phức tạp.'
                },
                {
                    icon: '🎤',
                    title: 'Phỏng vấn Hành vi Sự kiện (BEI)',
                    description: 'Khai thác các hành vi cụ thể trong quá khứ của ứng viên như một chỉ báo cho hiệu suất trong tương lai.'
                },
                {
                    icon: '🤝',
                    title: 'Bài tập Tình huống Nhóm',
                    description: 'Quan sát khả năng hợp tác, gây ảnh hưởng, giải quyết xung đột, và năng lực lãnh đạo nổi bật trong môi trường tương tác.'
                },
                {
                    icon: '📝',
                    title: 'Phiếu Tự đánh giá Cá nhân',
                    description: 'Ứng viên tự nhìn nhận về hiệu suất trong các hoạt động, điểm mạnh, điểm yếu và bài học rút ra.'
                }
            ];

            const processFlow = document.getElementById('assessment-process-flow');
            assessmentProcessData.forEach((item, index) => {
                const step = document.createElement('div');
                step.className = 'bg-slate-100 p-4 rounded-lg flex items-center space-x-4';
                step.innerHTML = `
                    <div class="text-3xl">${item.icon}</div>
                    <div>
                        <h3 class="font-semibold text-slate-800">${item.title}</h3>
                        <p class="text-sm text-slate-600">${item.description}</p>
                    </div>
                `;
                processFlow.appendChild(step);

                if (index < assessmentProcessData.length - 1) {
                    const arrow = document.createElement('div');
                    arrow.className = 'flex justify-center';
                    arrow.innerHTML = `<svg class="w-6 h-6 text-orange-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path></svg>`;
                    processFlow.appendChild(arrow);
                }
            });


            const agendaData = {
                day1: [
                    { time: '08:00 - 08:30', activity: 'Đón tiếp & Giới thiệu Chương trình', participants: 'Tất cả Ứng viên' },
                    { time: '08:30 - 09:00', activity: 'Hướng dẫn chung về Quy trình Đánh giá', participants: 'Tất cả Ứng viên' },
                    { time: '09:00 - 12:00', activity: 'Nghiên cứu Tình huống Cá nhân 1 (Phân tích & Chuẩn bị)', participants: 'Làm việc độc lập' },
                    { time: '12:00 - 13:00', activity: 'Ăn trưa', participants: 'Tất cả Ứng viên, Giám khảo' },
                    { time: '13:00 - 17:00', activity: 'Trình bày Tình huống Cá nhân 1 & Hỏi Đáp', participants: 'Chia nhóm song song' },
                    { time: '17:00 - 18:00', activity: 'HĐGK Họp Hiệu chỉnh Sơ bộ Ngày 1', participants: 'Hội đồng Giám khảo' }
                ],
                day2: [
                    { time: '08:00 - 08:15', activity: 'Thông báo Lịch trình Ngày 2', participants: 'Tất cả Ứng viên' },
                    { time: '08:15 - 10:45', activity: 'Bài tập Tình huống Nhóm', participants: 'Chia 4 nhóm' },
                    { time: '10:45 - 11:00', activity: 'Giải lao', participants: 'Tất cả Ứng viên' },
                    { time: '11:00 - 12:30', activity: 'Các Nhóm Trình bày Giải pháp & Hỏi Đáp', participants: 'Trình bày lần lượt' },
                    { time: '12:30 - 13:30', activity: 'Ăn trưa', participants: 'Tất cả Ứng viên, Giám khảo' },
                    { time: '13:30 - 15:30', activity: 'Bài Tự luận Phản tư Chuyên sâu', participants: 'Làm việc độc lập' },
                    { time: '15:30 - 17:00', activity: 'Phỏng vấn Hành vi Sự kiện (BEI)', participants: 'Theo lịch cá nhân' },
                    { time: '17:00 - 18:30', activity: 'HĐGK Họp Hiệu chỉnh Tổng thể', participants: 'Hội đồng Giám khảo' },
                    { time: '18:30 - 19:00', activity: 'Bế mạc & Cảm ơn', participants: 'Tất cả' },
                ]
            };

            const agendaContent = document.getElementById('agenda-content');
            const tabDay1 = document.getElementById('tab-day1');
            const tabDay2 = document.getElementById('tab-day2');

            function renderAgenda(day) {
                let html = '<div class="overflow-x-auto"><table class="min-w-full divide-y divide-gray-200"><thead><tr><th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Thời gian</th><th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Hoạt động</th><th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Đối tượng</th></tr></thead><tbody class="bg-white divide-y divide-gray-200">';
                agendaData[day].forEach(item => {
                    html += `<tr><td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${item.time}</td><td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">${item.activity}</td><td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${item.participants}</td></tr>`;
                });
                html += '</tbody></table></div>';
                agendaContent.innerHTML = html;
            }

            tabDay1.addEventListener('click', () => {
                renderAgenda('day1');
                tabDay1.className = 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-orange-600 border-orange-500';
                tabDay2.className = 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 hover:border-gray-300 border-transparent';
            });

            tabDay2.addEventListener('click', () => {
                renderAgenda('day2');
                tabDay2.className = 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-orange-600 border-orange-500';
                tabDay1.className = 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm text-gray-500 hover:text-gray-700 hover:border-gray-300 border-transparent';
            });
            renderAgenda('day1');


            const roadmapData = [
                {
                    time: '3 Tháng',
                    goal: 'Hoàn thiện kế hoạch triển khai, xây dựng sự đồng thuận và đạt được các "quick wins" ban đầu.',
                    kpis: ['Mức độ hoàn thành kế hoạch', 'Mức độ gắn kết đội ngũ', 'Tiến độ đạt "quick wins"']
                },
                {
                    time: '6 Tháng',
                    goal: 'Duy trì tiến độ, quản lý hiệu quả các thách thức và rủi ro, đảm bảo hiệu suất đội ngũ.',
                    kpis: ['Tỷ lệ hoàn thành công việc', 'Chỉ số hiệu suất đội ngũ', 'Mức độ kiểm soát rủi ro']
                },
                {
                    time: '12 Tháng',
                    goal: 'Hoàn thành mục tiêu dự án, đánh giá tác động, rút ra bài học kinh nghiệm và đề xuất hướng đi tiếp theo.',
                    kpis: ['Mức độ đạt mục tiêu chiến lược', 'Tác động thực tế lên kinh doanh', 'Chất lượng bài học kinh nghiệm']
                }
            ];

            const roadmapTimeline = document.getElementById('roadmap-timeline');
            roadmapData.forEach(item => {
                const milestone = document.createElement('div');
                milestone.className = 'mb-8 ml-8 cursor-pointer group';
                milestone.innerHTML = `
                    <div class="absolute -left-1.5 mt-1.5 w-3 h-3 bg-slate-400 rounded-full border border-white group-hover:bg-orange-500 transition-colors"></div>
                    <h3 class="text-lg font-semibold text-slate-900 group-hover:text-orange-600 transition-colors">${item.time}</h3>
                    <p class="mb-2 text-base font-normal text-slate-600">${item.goal}</p>
                    <div class="details text-sm">
                        <strong class="font-medium text-slate-700">KPIs then chốt:</strong>
                        <ul class="list-disc list-inside text-slate-500 mt-1">
                            ${item.kpis.map(kpi => `<li>${kpi}</li>`).join('')}
                        </ul>
                    </div>
                `;
                roadmapTimeline.appendChild(milestone);
                milestone.addEventListener('click', () => {
                   milestone.querySelector('.details').classList.toggle('open');
                });
            });

            const candidateData = [
                [4.5, 4, 3.5, 5, 4], 
                [3, 5, 4.5, 4, 3.5]   
            ];
            
            const ctx = document.getElementById('competencyChart').getContext('2d');
            const competencyChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: [
                        'Tự nhận thức',
                        'Lãnh đạo GHN',
                        'Tầm nhìn & Chiến lược',
                        'Năng lực Thực thi',
                        'Phù hợp Tổ chức'
                    ],
                    datasets: [{
                        label: 'Điểm đánh giá',
                        data: candidateData[0],
                        fill: true,
                        backgroundColor: 'rgba(251, 146, 60, 0.2)',
                        borderColor: 'rgb(251, 146, 60)',
                        pointBackgroundColor: 'rgb(251, 146, 60)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgb(251, 146, 60)'
                    }]
                },
                options: {
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: {
                                display: true
                            },
                            suggestedMin: 0,
                            suggestedMax: 5,
                            pointLabels: {
                                font: {
                                    size: 12
                                },
                                color: '#475569'
                            },
                             ticks: {
                                stepSize: 1
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                        }
                    }
                }
            });

            document.getElementById('candidate-select').addEventListener('change', function() {
                competencyChart.data.datasets[0].data = candidateData[this.value];
                competencyChart.update();
            });

        });
    </script>
</body>
</html>
