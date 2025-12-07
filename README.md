<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cửa Hàng Điện Tử - Thiết Bị Cao Cấp</title>
    <!-- Tải Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Cấu hình Tailwind -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'primary': '#3b82f6', // Xanh dương (dùng cho tiêu đề và các yếu tố phụ)
                        'secondary': '#10b981', // Xanh lá (dùng cho nút mua hàng/hành động chính)
                        'dark-bg': '#1f2937',
                        'dark-card': '#374151',
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style>
        /* Thiết lập font Inter cho toàn bộ trang */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Hiệu ứng focus cho thanh tìm kiếm */
        #search-input:focus {
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5);
            border-color: #3b82f6;
        }
    </style>
</head>
<body class="min-h-screen">

    <!-- Header -->
    <header class="bg-dark-bg text-white shadow-lg sticky top-0 z-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <!-- Đã thay đổi tên cửa hàng thành https.store -->
            <h1 class="text-3xl font-extrabold tracking-tight">
                <span class="text-primary">https</span>.store
            </h1>
            <nav class="hidden md:block">
                <a href="#" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition duration-150">Trang Chủ</a>
                <a href="#" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition duration-150">Sản Phẩm</a>
                <a href="#" class="text-gray-300 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition duration-150">Liên Hệ</a>
            </nav>
            <div class="relative">
                <button class="bg-primary hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition duration-150 transform hover:scale-105">
                    Giỏ Hàng (0)
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <h2 class="text-4xl font-bold text-gray-900 mb-6 border-b pb-2">Danh Sách Sản Phẩm</h2>

        <!-- Search and Filter Bar -->
        <div class="mb-8 flex flex-col md:flex-row gap-4">
            <div class="relative w-full md:w-2/3">
                <input type="text" id="search-input" placeholder="Tìm kiếm sản phẩm... (Tên, mô tả, danh mục)"
                       class="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-xl focus:outline-none transition duration-150 text-lg shadow-sm">
                <svg class="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            </div>

            <!-- Nút Lọc (Tùy chọn mở rộng cho lọc nâng cao) -->
            <button class="w-full md:w-1/3 bg-gray-200 text-gray-700 font-semibold py-3 px-4 rounded-xl shadow-sm hover:bg-gray-300 transition duration-150">
                Lọc Nâng Cao
            </button>
        </div>

        <!-- Product Grid -->
        <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            <!-- Sản phẩm sẽ được render bởi JavaScript tại đây -->
        </div>

        <!-- No Results Message -->
        <div id="no-results" class="hidden text-center py-20">
            <svg class="mx-auto h-12 w-12 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 19.414A7 7 0 0112 11a7 7 0 010 14m0 0a9.003 9.003 0 01-4.908-1.442M12 12a2 2 0 100-4 2 2 0 000 4z"></path></svg>
            <h3 class="mt-2 text-sm font-medium text-gray-900">Không tìm thấy sản phẩm nào</h3>
            <p class="mt-1 text-sm text-gray-500">
                Vui lòng thử lại với từ khóa khác.
            </p>
        </div>
    </main>

    <!-- Footer đã được cập nhật để loại bỏ dòng "Thiết kế bởi Gemini AI" và sử dụng màu nền đồng nhất -->
    <footer class="bg-dark-bg text-white mt-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center text-sm">
            &copy; 2024 https.store. Bảo mật & Cao cấp.
        </div>
    </footer>

    <!-- MODAL HIỂN THỊ THÔNG TIN CHI TIẾT SẢN PHẨM -->
    <div id="product-modal" class="fixed inset-0 z-50 overflow-y-auto hidden" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <!-- Overlay backdrop, đóng modal khi click ra ngoài -->
        <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0" onclick="closeProductModal()">
            <div class="fixed inset-0 bg-gray-900 bg-opacity-75 transition-opacity" aria-hidden="true"></div>

            <!-- Modal Panel (ngăn chặn đóng khi click vào nội dung) -->
            <div class="inline-block align-bottom bg-white rounded-xl text-left overflow-hidden shadow-xl transform transition-all my-8 sm:align-middle w-full max-w-4xl" onclick="event.stopPropagation()">
                <div class="bg-white p-6 sm:p-8">
                    <!-- Nút đóng -->
                    <button type="button" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 transition duration-150" onclick="closeProductModal()">
                        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
                    </button>

                    <!-- Nội dung chi tiết được render bởi JS -->
                    <div id="modal-content">
                        <!-- Nội dung sẽ được chèn vào đây bởi JavaScript -->
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- KẾT THÚC MODAL -->

    <script>
        // --- DỮ LIỆU SẢN PHẨM MẪU ---
        const products = [
            {
                id: 1,
                name: "Laptop Gaming ASUS ROG Strix",
                category: "Máy tính xách tay",
                price: 35500000,
                description: "Hiệu năng cực đỉnh với chip Core i9 thế hệ mới và card đồ họa RTX 4080. Màn hình 16 inch QHD 240Hz.",
                image: "https://placehold.co/400x300/4f46e5/ffffff?text=LAPTOP+GAMING",
                longDescription: "Dòng ROG Strix được thiết kế cho các game thủ chuyên nghiệp, kết hợp giữa tốc độ xử lý nhanh chóng và hệ thống tản nhiệt tiên tiến. Chiếc laptop này là sự lựa chọn hoàn hảo cho bất kỳ tựa game AAA nào."
            },
            {
                id: 2,
                name: "Điện thoại Samsung Galaxy S24 Ultra",
                category: "Điện thoại",
                price: 28990000,
                description: "Camera 200MP, S Pen tích hợp, và sức mạnh từ chip Snapdragon 8 Gen 3 for Galaxy. Trải nghiệm AI tiên tiến.",
                image: "https://placehold.co/400x300/059669/ffffff?text=GALAXY+S24",
                longDescription: "Samsung Galaxy S24 Ultra định nghĩa lại chuẩn mực của điện thoại thông minh. Với bộ xử lý độc quyền và các tính năng AI đột phá, nó mang lại trải nghiệm sáng tạo và năng suất chưa từng có."
            },
            {
                id: 3,
                name: "Tai nghe không dây Sony WH-1000XM5",
                category: "Âm thanh",
                price: 7990000,
                description: "Chống ồn chủ động hàng đầu, chất lượng âm thanh Hi-Res Audio, thời lượng pin lên đến 30 giờ.",
                image: "https://placehold.co/400x300/f59e0b/ffffff?text=HEADPHONE",
                longDescription: "Tai nghe Sony WH-1000XM5 mang đến sự kết hợp hoàn hảo giữa công nghệ chống ồn tuyệt vời và âm thanh chi tiết, phù hợp cho những người yêu nhạc khó tính và cần sự yên tĩnh tối đa."
            },
            {
                id: 4,
                name: "Màn hình cong Dell UltraSharp 34 inch",
                category: "Màn hình",
                price: 15490000,
                description: "Độ phân giải WQHD, tấm nền IPS, thiết kế viền mỏng ấn tượng. Lý tưởng cho công việc và giải trí.",
                image: "https://placehold.co/400x300/ef4444/ffffff?text=MONITOR",
                longDescription: "Màn hình cong UltraSharp 34 inch nâng cao trải nghiệm làm việc đa nhiệm và giải trí đắm chìm. Công nghệ ComfortView Plus giảm ánh sáng xanh mà vẫn giữ được độ chính xác màu sắc tuyệt vời."
            },
            {
                id: 5,
                name: "Máy ảnh Mirrorless Canon EOS R6 Mark II",
                category: "Máy ảnh",
                price: 65000000,
                description: "Cảm biến 24.2MP full-frame, quay video 4K 60p không crop. Tốc độ chụp liên tục nhanh chóng.",
                image: "https://placehold.co/400x300/8b5cf6/ffffff?text=CAMERA",
                longDescription: "EOS R6 Mark II là một chiếc máy ảnh lai mạnh mẽ, kết hợp khả năng chụp ảnh tĩnh tốc độ cao với khả năng quay video chất lượng điện ảnh, là công cụ không thể thiếu cho các nhà sáng tạo nội dung chuyên nghiệp."
            },
            {
                id: 6,
                name: "Đồng hồ thông minh Apple Watch Series 9",
                category: "Thiết bị đeo",
                price: 11990000,
                description: "Màn hình luôn bật sáng hơn, chip S9 SiP mạnh mẽ, và tính năng Double Tap độc đáo.",
                image: "https://placehold.co/400x300/14b8a6/ffffff?text=APPLE+WATCH",
                longDescription: "Apple Watch Series 9 là đối tác hoàn hảo cho sức khỏe và thể chất, cung cấp các cảm biến tiên tiến để theo dõi hoạt động, giấc ngủ và các chỉ số sức khỏe quan trọng khác một cách chính xác."
            },
            {
                id: 7,
                name: "Máy chơi game console Sony PlayStation 5 Slim",
                category: "Gaming",
                price: 14500000,
                description: "Trải nghiệm chơi game 4K 120Hz, công nghệ âm thanh 3D Audio và SSD siêu tốc.",
                image: "https://placehold.co/400x300/ec4899/ffffff?text=PS5+SLIM",
                longDescription: "PlayStation 5 Slim cung cấp hiệu suất tuyệt vời trong một thiết kế nhỏ gọn hơn. Tận hưởng thời gian tải gần như tức thì và độ sâu hình ảnh đáng kinh ngạc trong các trò chơi độc quyền."
            },
            {
                id: 8,
                name: "Loa thông minh Google Nest Hub 2nd Gen",
                category: "Nhà thông minh",
                price: 2800000,
                description: "Màn hình 7 inch hiển thị thông tin, điều khiển thiết bị nhà thông minh bằng giọng nói.",
                image: "https://placehold.co/400x300/6b7280/ffffff?text=NEST+HUB",
                longDescription: "Google Nest Hub 2nd Gen là trung tâm điều khiển nhà thông minh của bạn. Giúp bạn xem lịch, phát nhạc, điều khiển đèn và thậm chí theo dõi giấc ngủ của bạn chỉ bằng một màn hình cảm ứng hoặc lệnh thoại."
            }
        ];

        // Hàm định dạng tiền tệ Việt Nam
        function formatCurrency(amount) {
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
        }

        // Hàm render (hiển thị) sản phẩm ra giao diện
        function renderProducts(filteredProducts) {
            const grid = document.getElementById('product-grid');
            const noResults = document.getElementById('no-results');
            grid.innerHTML = '';

            if (filteredProducts.length === 0) {
                noResults.classList.remove('hidden');
                return;
            }

            noResults.classList.add('hidden');

            filteredProducts.forEach(product => {
                // Thêm thẻ "HOT" cho sản phẩm đầu tiên để làm cho nó nổi bật hơn
                const isHot = product.id === 1;
                const badgeHtml = isHot ?
                    `<span class="absolute top-0 right-0 bg-red-600 text-white text-xs font-bold px-3 py-1 rounded-bl-xl z-10 shadow-lg transform rotate-3 scale-105">HOT</span>`
                    : '';
                
                // Thêm sự kiện onclick vào thẻ sản phẩm để mở modal
                const productCard = `
                    <div class="relative bg-white rounded-xl shadow-xl hover:shadow-2xl transition duration-300 transform hover:-translate-y-1 overflow-hidden border border-gray-100 cursor-pointer" onclick="openProductModal(${product.id})">
                        ${badgeHtml}
                        <!-- Hình ảnh sản phẩm -->
                        <div class="h-48 overflow-hidden">
                             <img src="${product.image}" onerror="this.onerror=null;this.src='https://placehold.co/400x300/CCCCCC/333333?text=Khong+Co+Anh';" alt="${product.name}" class="w-full h-full object-cover">
                        </div>

                        <div class="p-5">
                            <!-- Danh mục -->
                            <span class="text-xs font-semibold uppercase tracking-wider text-primary bg-blue-50 px-2 py-1 rounded-full">${product.category}</span>

                            <!-- Tên sản phẩm -->
                            <h3 class="text-xl font-bold text-gray-900 mt-2 mb-2 truncate">${product.name}</h3>

                            <!-- Mô tả ngắn -->
                            <p class="text-gray-600 text-sm mb-4 line-clamp-2">${product.description}</p>

                            <!-- Giá và nút mua hàng -->
                            <div class="flex items-center justify-between">
                                <!-- Giá được làm nổi bật với màu đỏ và cỡ chữ lớn -->
                                <span class="text-3xl font-extrabold text-red-600">${formatCurrency(product.price)}</span>
                                <!-- Nút này không có onclick để tránh xung đột với mở modal, nó chỉ là UI -->
                                <button class="bg-secondary hover:bg-green-600 text-white font-medium py-2 px-5 rounded-full text-md transition duration-150 transform hover:scale-105 shadow-xl shadow-secondary/50">
                                    Thêm vào Giỏ
                                </button>
                            </div>
                        </div>
                    </div>
                `;
                grid.innerHTML += productCard;
            });
        }

        // Hàm xử lý tìm kiếm và lọc
        function handleSearch() {
            const query = document.getElementById('search-input').value.toLowerCase().trim();

            const filtered = products.filter(product => {
                // Kết hợp các trường cần tìm kiếm thành một chuỗi lớn để lọc
                const searchableText = `${product.name} ${product.description} ${product.category}`.toLowerCase();
                
                // Trả về true nếu chuỗi tìm kiếm (query) nằm trong chuỗi searchableText
                return searchableText.includes(query);
            });

            renderProducts(filtered);
        }

        // Kỹ thuật Debounce để tối ưu hiệu suất (chỉ gọi hàm sau khi người dùng ngừng gõ một lúc)
        function debounce(func, delay) {
            let timeout;
            return function(...args) {
                clearTimeout(timeout);
                timeout = setTimeout(() => func.apply(this, args), delay);
            };
        }

        // Hàm mở modal thông tin chi tiết sản phẩm
        function openProductModal(id) {
            const product = products.find(p => p.id === id);
            if (!product) return;

            const modal = document.getElementById('product-modal');
            const modalContent = document.getElementById('modal-content');

            // Tạo nội dung chi tiết cho modal
            modalContent.innerHTML = `
                <div class="md:flex md:items-start">
                    <!-- Hình ảnh chi tiết (Trái) -->
                    <div class="md:w-1/2 p-4 flex justify-center items-center">
                        <img src="${product.image.replace('400x300', '600x450')}" alt="${product.name}" class="w-full h-auto object-cover rounded-xl shadow-2xl max-h-[450px]">
                    </div>

                    <!-- Thông tin sản phẩm (Phải) -->
                    <div class="md:w-1/2 p-4 md:pt-8">
                        <span class="text-sm font-semibold uppercase tracking-wider text-primary bg-blue-50 px-3 py-1 rounded-full">${product.category}</span>
                        
                        <h2 class="text-4xl font-extrabold text-gray-900 mt-3 mb-4">${product.name}</h2>
                        
                        <!-- Mô tả dài hơn (sử dụng longDescription nếu có) -->
                        <p class="text-gray-700 text-lg mb-6">${product.longDescription || product.description}</p>

                        <div class="border-t border-b py-4 mb-6">
                            <span class="text-sm font-semibold text-gray-500 block">Giá bán lẻ đề nghị:</span>
                            <div class="flex items-baseline">
                                <span class="text-2xl font-semibold text-gray-500 line-through mr-4">${formatCurrency(product.price * 1.15)}</span>
                                <span class="text-5xl font-extrabold text-red-600">${formatCurrency(product.price)}</span>
                            </div>
                            <p class="text-green-600 font-bold text-sm mt-2">Đang có chương trình khuyến mãi đặc biệt!</p>
                        </div>

                        <!-- Action buttons -->
                        <div class="flex space-x-4">
                            <button class="flex-1 bg-secondary hover:bg-green-600 text-white font-bold py-3 px-6 rounded-full text-lg transition duration-150 transform hover:scale-105 shadow-xl shadow-secondary/50">
                                Thêm vào Giỏ Hàng
                            </button>
                            <button class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-semibold py-3 px-6 rounded-full transition duration-150">
                                Liên hệ tư vấn
                            </button>
                        </div>
                    </div>
                </div>
            `;
            modal.classList.remove('hidden');
        }

        // Hàm đóng modal
        function closeProductModal() {
            document.getElementById('product-modal').classList.add('hidden');
        }

        // Gán sự kiện cho thanh tìm kiếm với debounce và đóng modal bằng phím ESC
        document.addEventListener('DOMContentLoaded', () => {
            // Hiển thị tất cả sản phẩm khi trang vừa tải
            renderProducts(products);

            // Gắn sự kiện input với debounce 300ms
            const searchInput = document.getElementById('search-input');
            const debouncedSearch = debounce(handleSearch, 300);
            searchInput.addEventListener('input', debouncedSearch);

            // Đóng modal khi nhấn phím ESC
            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    closeProductModal();
                }
            });
        });

    </script>
</body>
</html>
