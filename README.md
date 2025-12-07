<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản Lý Sản Phẩm Dark Mode Premium</title>
    <!-- Tải Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* CSS tuỳ chỉnh cho giao diện Dark Mode Premium */
        body {
            font-family: 'Inter', sans-serif;
            /* Màu nền ngoài container chính: Xám đen sâu */
            background-color: #1f2937; /* slate-800 */
        }
        
        /* Màu sắc cho trạng thái sản phẩm (giữ nguyên để phân biệt rõ ràng) */
        .status-badge {
            padding: 4px 8px;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
        .status-con-hang {
            background-color: #065f46; /* green-800 */
            color: #d1fae5; /* green-100 */
        }
        .status-het-hang {
            background-color: #991b1b; /* red-800 */
            color: #fee2e2; /* red-100 */
        }
        .status-dang-ve {
            background-color: #92400e; /* amber-800 */
            color: #fef3c7; /* amber-100 */
        }
        
        /* Hiệu ứng tương tác cho hàng trong bảng */
        .clickable-row {
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            /* Màu chữ mặc định trong bảng sẽ là màu sáng (text-gray-300) */
        }
        .clickable-row:hover {
            /* Hiệu ứng hover sang trọng: nền hổ phách đậm hơn */
            background-color: #4b5563 !important; /* gray-600 */
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(255, 255, 255, 0.05), 0 4px 6px -4px rgba(255, 255, 255, 0.05);
        }
        /* Style cho hàng sọc ngựa vằn (Đổi màu cho Dark Mode) */
        #productTableBody tr {
            background-color: #374151; /* gray-700 */
            color: #d1d5db; /* gray-300 */
        }
        #productTableBody tr:nth-child(even) {
            background-color: #1f2937; /* gray-800 (tối hơn) */
        }
        
        /* Style cho hình ảnh sản phẩm */
        .product-image {
            width: 50px;
            height: 50px;
            object-fit: cover;
            border-radius: 6px;
            box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.5);
        }
    </style>
</head>

<body class="p-4 sm:p-10">

    <!-- Container Chính với Shadow Mờ Sang Trọng -->
    <div class="max-w-7xl mx-auto bg-gray-900 shadow-2xl shadow-amber-500/50 rounded-3xl p-8 md:p-12 border border-amber-500/10">

        <!-- Header -->
        <div class="border-b border-amber-500/20 pb-4 mb-8">
            <h1 class="text-4xl font-extrabold text-white">Quản Lý Dữ Liệu Sản Phẩm <span class="text-amber-500">Premium</span></h1>
            <p class="text-gray-400 mt-2">Nền tảng quản lý với giao diện đẳng cấp và chức năng toàn diện.</p>
        </div>

        <!-- Control Panel: Search & Filter -->
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
            
            <!-- Input Tìm kiếm Chính -->
            <div class="relative col-span-1 md:col-span-2 shadow-lg rounded-lg">
                <input
                    type="text"
                    id="searchInput"
                    placeholder="Tìm kiếm theo Tên, Mã hoặc Mô tả sản phẩm..."
                    class="w-full p-3 pl-10 border border-amber-600/50 rounded-lg focus:ring-4 focus:ring-amber-200 focus:border-amber-500 transition duration-300 shadow-inner bg-gray-800 text-white"
                >
                <!-- Icon Kính lúp (Màu nhấn Amber) -->
                <svg class="absolute left-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            </div>

            <!-- Dropdown Lọc Tình trạng -->
            <select
                id="statusFilter"
                class="col-span-1 p-3 border border-amber-600/50 rounded-lg bg-gray-800 focus:ring-4 focus:ring-amber-200 focus:border-amber-500 transition duration-300 shadow-lg text-white"
                onchange="updateTable()"
            >
                <option value="Tất cả">Lọc theo Tình trạng (Tất cả)</option>
                <option value="Còn hàng">Còn hàng</option>
                <option value="Hết hàng">Hết hàng</option>
                <option value="Đang về">Đang về</option>
            </select>

            <!-- Dropdown Lọc Giá Bán (Khoảng giá) -->
            <select
                id="priceFilter"
                class="col-span-1 p-3 border border-amber-600/50 rounded-lg bg-gray-800 focus:ring-4 focus:ring-amber-200 focus:border-amber-500 transition duration-300 shadow-lg text-white"
                onchange="updateTable()"
            >
                <option value="all">Lọc theo Giá bán (Tất cả)</option>
                <option value="0-5000000">Dưới 5 Triệu đ</option>
                <option value="5000000-15000000">5 Triệu đ - 15 Triệu đ</option>
                <option value="15000000-max">Trên 15 Triệu đ</option>
            </select>

        </div>

        <!-- Data Table -->
        <div class="overflow-x-auto rounded-xl shadow-2xl border border-amber-500/20">
            <table class="min-w-full divide-y divide-gray-700">
                <!-- Header Bảng: Màu tối sang trọng -->
                <thead class="bg-slate-900">
                    <tr>
                        <th class="px-6 py-4 text-left text-xs font-bold text-amber-500 uppercase tracking-wider rounded-tl-xl">Hình Ảnh</th>
                        <!-- Header Sắp xếp theo Tên -->
                        <th id="sortName" onclick="requestSort('name')" 
                            class="px-6 py-4 text-left text-xs font-bold text-white uppercase tracking-wider cursor-pointer hover:bg-slate-800 transition duration-150 flex items-center">
                            Tên Sản Phẩm <span id="sort-icon-name" class="ml-2"></span>
                        </th>
                        <th class="px-6 py-4 text-left text-xs font-bold text-white uppercase tracking-wider">Mã SP</th>
                        <!-- Header Sắp xếp theo Giá Bán -->
                        <th id="sortPrice" onclick="requestSort('price')" 
                            class="px-6 py-4 text-left text-xs font-bold text-white uppercase tracking-wider cursor-pointer hover:bg-slate-800 transition duration-150 flex items-center">
                            Giá Bán <span id="sort-icon-price" class="ml-2"></span>
                        </th>
                        <th class="px-6 py-4 text-left text-xs font-bold text-white uppercase tracking-wider rounded-tr-xl">Tình Trạng</th>
                    </tr>
                </thead>
                <tbody id="productTableBody">
                    <!-- Data rows: Giá trị Price được làm nổi bật với màu Amber -->
                    <tr class="clickable-row" data-name="Laptop ASUS Zenbook" data-code="LPT001" data-status="Còn hàng" data-description="Laptop cao cấp, mỏng nhẹ, hiệu năng vượt trội cho công việc đồ họa." data-quantity="50" data-price-value="25000000" data-image="https://placehold.co/50x50/374151/facc15?text=Zen" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Zen" class="product-image" alt="Laptop ASUS Zenbook"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Laptop ASUS Zenbook</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">LPT001</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">25,000,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-con-hang">Còn hàng</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Điện Thoại Samsung Galaxy" data-code="DT005" data-status="Còn hàng" data-description="Điện thoại thông minh Android, màn hình AMOLED, camera 108MP." data-quantity="120" data-price-value="18500000" data-image="https://placehold.co/50x50/374151/facc15?text=Sam" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Sam" class="product-image" alt="Điện Thoại Samsung Galaxy"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Điện Thoại Samsung Galaxy</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">DT005</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">18,500,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-con-hang">Còn hàng</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Bàn Phím Cơ Logitech" data-code="BP012" data-status="Hết hàng" data-description="Bàn phím cơ full-size, switch Brown, thích hợp cho cả gõ văn bản và chơi game." data-quantity="0" data-price-value="1950000" data-image="https://placehold.co/50x50/374151/facc15?text=Key" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Key" class="product-image" alt="Bàn Phím Cơ Logitech"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Bàn Phím Cơ Logitech</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">BP012</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">1,950,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-het-hang">Hết hàng</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Chuột Không Dây Gaming" data-code="CT008" data-status="Đang về" data-description="Chuột không dây chuyên nghiệp, độ phân giải cao, pin 100 giờ. Thiết kế công thái học." data-quantity="35" data-price-value="850000" data-image="https://placehold.co/50x50/374151/facc15?text=Mse" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Mse" class="product-image" alt="Chuột Không Dây Gaming"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Chuột Không Dây Gaming</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">CT008</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">850,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-dang-ve">Đang về</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Màn Hình 4K Dell" data-code="MH003" data-status="Còn hàng" data-description="Màn hình 27 inch 4K, tấm nền IPS, màu sắc chuẩn cho thiết kế đồ họa." data-quantity="75" data-price-value="12500000" data-image="https://placehold.co/50x50/374151/facc15?text=Mon" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Mon" class="product-image" alt="Màn Hình 4K Dell"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Màn Hình 4K Dell</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">MH003</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">12,500,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-con-hang">Còn hàng</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Tai Nghe Chống Ồn" data-code="TN010" data-status="Hết hàng" data-description="Tai nghe chụp tai, công nghệ chống ồn chủ động (ANC) hàng đầu. Âm thanh Hi-Fi." data-quantity="0" data-price-value="3500000" data-image="https://placehold.co/50x50/374151/facc15?text=Hst" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Hst" class="product-image" alt="Tai Nghe Chống Ồn"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Tai Nghe Chống Ồn</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">TN010</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">3,500,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-het-hang">Hết hàng</span></td>
                    </tr>
                    <tr class="clickable-row" data-name="Webcam 4K Cao Cấp" data-code="WC004" data-status="Đang về" data-description="Webcam độ phân giải 4K, tích hợp AI auto-focus. Hoàn hảo cho video call chất lượng cao." data-quantity="20" data-price-value="4800000" data-image="https://placehold.co/50x50/374151/facc15?text=Web" onclick="showProductDetails(this)">
                        <td class="px-6 py-4 whitespace-nowrap"><img src="https://placehold.co/50x50/374151/facc15?text=Web" class="product-image" alt="Webcam 4K Cao Cấp"></td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-amber-500 hover:text-amber-400 underline cursor-pointer">Webcam 4K Cao Cấp</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">WC004</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-amber-500 font-extrabold">4,800,000₫</td>
                        <td class="px-6 py-4 whitespace-nowrap"><span class="status-badge status-dang-ve">Đang về</span></td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Footer Info -->
        <p id="resultCount" class="mt-6 text-sm text-gray-400">Hiển thị 7 kết quả.</p>

    </div>

    <!-- Modal Hiển Thị Chi Tiết Sản Phẩm -->
    <div id="productModal" class="fixed inset-0 bg-gray-900 bg-opacity-70 hidden items-center justify-center z-50 transition-opacity duration-300">
        <div class="bg-gray-800 rounded-xl shadow-2xl shadow-amber-500/50 w-full max-w-lg mx-4 transform transition-all duration-300 scale-100 p-6 md:p-8">

            <!-- Modal Header -->
            <div class="flex justify-between items-center border-b border-amber-200/50 pb-3 mb-4">
                <h2 id="modalProductName" class="text-2xl font-bold text-white">Chi Tiết Sản Phẩm</h2>
                <button onclick="closeModal()" class="text-gray-400 hover:text-amber-600 transition">
                    <!-- Icon Đóng (Close) -->
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                </button>
            </div>
            
            <!-- Modal Body (Details) -->
            <div class="space-y-4 text-gray-300">
                <!-- Hiển thị ảnh lớn hơn trong Modal -->
                <div class="flex justify-center mb-4">
                    <img id="modalProductImage" src="" alt="Ảnh sản phẩm" class="w-32 h-32 object-cover rounded-lg shadow-xl border border-amber-500">
                </div>

                <p><strong>Mã Sản Phẩm:</strong> <span id="modalProductCode" class="font-semibold text-white"></span></p>
                <p><strong>Giá Bán:</strong> <span id="modalProductPrice" class="font-bold text-amber-500 text-xl"></span></p>
                <p><strong>Tình Trạng:</strong> <span id="modalProductStatus" class="font-semibold"></span></p>
                <p><strong>Số Lượng Tồn Kho:</strong> <span id="modalProductQuantity" class="font-semibold"></span></p>
                <div class="pt-2">
                    <p class="font-bold text-white mb-1">Mô Tả:</p>
                    <p id="modalProductDescription" class="text-sm italic border-l-4 border-amber-400 pl-3 py-2 bg-gray-900 rounded-md shadow-inner"></p>
                </div>
            </div>

            <!-- Modal Footer -->
            <div class="mt-6 pt-4 border-t border-amber-200/50 flex justify-end">
                <button onclick="closeModal()" class="px-8 py-2 bg-amber-500 text-gray-900 font-medium rounded-lg hover:bg-amber-600 transition shadow-lg transform hover:scale-[1.02]">Đóng</button>
            </div>

        </div>
    </div>

    <script>
        // State để quản lý trạng thái sắp xếp
        let sortState = {
            column: null, // 'name' hoặc 'price'
            direction: 'asc' // 'asc' hoặc 'desc'
        };

        // Hàm cập nhật số lượng kết quả hiển thị
        function updateResultCount(count) {
            const resultCount = document.getElementById('resultCount');
            const totalRows = document.getElementById('productTableBody').getElementsByTagName('tr').length;
            const visibleCount = (typeof count !== 'undefined') ? count : totalRows;

            resultCount.textContent = visibleCount === 0
                ? "Không tìm thấy kết quả nào."
                : `Hiển thị ${visibleCount} kết quả.`;

            // Màu chữ kết quả cho Dark Mode
            resultCount.classList.toggle('text-red-500', visibleCount === 0);
            resultCount.classList.toggle('text-gray-400', visibleCount !== 0);
        }

        // Hàm cập nhật icon sắp xếp trên tiêu đề
        const updateSortIcons = () => {
            // Xóa hết icon cũ
            document.getElementById('sort-icon-name').innerHTML = '';
            document.getElementById('sort-icon-price').innerHTML = '';

            // Icon SVG cho tăng dần và giảm dần (Màu trắng cho header tối)
            const iconAsc = `<svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline-block align-middle text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M5 15l7-7 7 7" /></svg>`;
            const iconDesc = `<svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline-block align-middle text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" /></svg>`;
            
            if (sortState.column) {
                const iconSpan = document.getElementById(`sort-icon-${sortState.column}`);
                if (iconSpan) {
                    iconSpan.innerHTML = sortState.direction === 'asc' ? iconAsc : iconDesc;
                }
            }
        }

        // Hàm yêu cầu sắp xếp khi click vào tiêu đề
        function requestSort(column) {
            // 1. Xác định hướng sắp xếp mới
            if (sortState.column === column) {
                sortState.direction = sortState.direction === 'asc' ? 'desc' : 'asc';
            } else {
                sortState.column = column;
                sortState.direction = 'asc'; // Mặc định tăng dần khi đổi cột
            }
            
            // 2. Cập nhật icon và bảng
            updateSortIcons();
            updateTable();
        }

        // Hàm chính để cập nhật, lọc và sắp xếp bảng
        const updateTable = () => {
            // 1. Lấy dữ liệu từ Control Panel
            const keyword = document.getElementById('searchInput').value.trim().toLowerCase();
            const filterStatus = document.getElementById('statusFilter').value;
            const filterPriceRange = document.getElementById('priceFilter').value;

            // 2. Lấy TẤT CẢ các dòng (rows) từ DOM
            const allRowsInDOM = Array.from(document.getElementById('productTableBody').getElementsByTagName('tr'));
            
            let visibleRowCount = 0;

            // 3. Áp dụng LỌC (Filtering)
            allRowsInDOM.forEach(row => {
                // Lấy dữ liệu từ data attributes
                const name = row.getAttribute('data-name').toLowerCase();
                const code = row.getAttribute('data-code').toLowerCase();
                const description = row.getAttribute('data-description').toLowerCase();
                const status = row.getAttribute('data-status');
                const priceValue = parseInt(row.getAttribute('data-price-value'));

                // Kiểm tra Tìm kiếm (Tên HOẶC Mã HOẶC Mô tả)
                const matchesSearch = name.includes(keyword) || code.includes(keyword) || description.includes(keyword);

                // Kiểm tra Lọc Tình trạng
                const matchesStatusFilter = filterStatus === 'Tất cả' || status === filterStatus;

                // Kiểm tra Lọc Giá Bán
                let matchesPriceFilter = true;
                if (filterPriceRange !== 'all') {
                    const [minStr, maxStr] = filterPriceRange.split('-');
                    const minPrice = parseInt(minStr);
                    const maxPrice = maxStr === 'max' ? Infinity : parseInt(maxStr);

                    matchesPriceFilter = priceValue >= minPrice && priceValue <= maxPrice;
                }

                // Quyết định hiển thị (Search AND Status AND Price)
                const isVisible = matchesSearch && matchesStatusFilter && matchesPriceFilter;

                row.style.display = isVisible ? "" : "none";
                if (isVisible) visibleRowCount++;
            });

            // 4. Áp dụng SẮP XẾP (Sorting) cho TẤT CẢ các dòng
            if (sortState.column) {
                allRowsInDOM.sort((rowA, rowB) => {
                    let valA, valB;
                    let comparison = 0;

                    if (sortState.column === 'name') {
                        valA = rowA.getAttribute('data-name').toLowerCase();
                        valB = rowB.getAttribute('data-name').toLowerCase();
                        // So sánh chuỗi
                        if (valA > valB) comparison = 1;
                        else if (valA < valB) comparison = -1;
                    } else if (sortState.column === 'price') {
                        // So sánh số
                        valA = parseInt(rowA.getAttribute('data-price-value'));
                        valB = parseInt(rowB.getAttribute('data-price-value'));
                        comparison = valA - valB; 
                    }

                    // Đảo ngược kết quả nếu là sắp xếp giảm dần
                    return sortState.direction === 'asc' ? comparison : comparison * -1;
                });
            }

            // 5. RENDER lại bảng (chèn các dòng đã được sắp xếp trở lại DOM)
            const tableBody = document.getElementById('productTableBody');
            allRowsInDOM.forEach(row => tableBody.appendChild(row));

            // 6. Cập nhật số lượng kết quả
            updateResultCount(visibleRowCount);
        };

        // Hàm hiển thị chi tiết sản phẩm (Đã cập nhật thêm hình ảnh)
        function showProductDetails(row) {
            const name = row.getAttribute('data-name');
            const code = row.getAttribute('data-code');
            const status = row.getAttribute('data-status');
            const price = row.querySelector('td:nth-child(4)').textContent; 
            const description = row.getAttribute('data-description');
            const quantity = row.getAttribute('data-quantity');
            const imageURL = row.getAttribute('data-image'); // Lấy URL ảnh

            // Cập nhật nội dung Modal
            document.getElementById('modalProductName').textContent = name;
            document.getElementById('modalProductCode').textContent = code;
            document.getElementById('modalProductPrice').textContent = price;
            document.getElementById('modalProductStatus').textContent = status;
            document.getElementById('modalProductQuantity').textContent = quantity;
            document.getElementById('modalProductDescription').textContent = description;
            document.getElementById('modalProductImage').src = imageURL; // Cập nhật ảnh trong Modal

            // Hiển thị Modal
            const modal = document.getElementById('productModal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
            document.body.style.overflow = 'hidden';
        }

        // Hàm đóng Modal (giữ nguyên)
        function closeModal() {
            const modal = document.getElementById('productModal');
            modal.classList.remove('flex');
            modal.classList.add('hidden');
            document.body.style.overflow = '';
        }

        // Thiết lập Event Listeners khi DOM đã tải xong
        document.addEventListener('DOMContentLoaded', () => {
            // Bắt sự kiện thay đổi cho Tìm kiếm và Lọc
            document.getElementById('searchInput').addEventListener('input', updateTable);
            document.getElementById('statusFilter').addEventListener('change', updateTable);
            document.getElementById('priceFilter').addEventListener('change', updateTable);

            // Khởi tạo bảng lần đầu
            updateTable();
            updateSortIcons(); // Khởi tạo icon sắp xếp
        });
    </script>

</body>
</html>
