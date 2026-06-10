
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FM Collection</title>
    <!-- Favicon Aplikasi (Menggunakan Logo FM Collection) -->
    <link class="no-print" rel="icon" type="image/png" href="https://iili.io/CBfHoR1.png">
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Premium (Inter, Montserrat, Roboto, Playfair Display, Pacifico, Lora, Oswald, Fredoka, Lobster, Caveat) -->
    <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@400;700&family=Fredoka:wght@400;600;700&family=Inter:wght@300;400;500;600;700&family=Lobster&family=Lora:ital,wght@0,400;0,700;1,400&family=Montserrat:ital,wght@0,400;0,700;1,400&family=Oswald:wght@400;700&family=Pacifico&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1e1e1e;
            color: #e3e3e3;
            overflow: hidden;
        }
        /* Skrol gaya premium gelap khas Photoshop */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #1e1e1e;
        }
        ::-webkit-scrollbar-thumb {
            background: #444;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #555;
        }
        
        /* Tetapan Cetakan Fizikal Presisi Tinggi (Bebas dari Area Hitam / Latar Belakang Bocor) */
        @media print {
            /* Sembunyikan semua elemen UI kecuali area print */
            header, aside, .no-print, .photo-actions, .resize-handle, .node-handle {
                display: none !important;
            }
            
            /* Sembunyikan garis seleksi putus-putus biru semasa cetak */
            .photo-wrapper {
                border: none !important;
                outline: none !important;
                box-shadow: none !important;
            }
            .photo-wrapper.selected {
                border: none !important;
                outline: none !important;
            }
            
            /* Pastikan latar belakang cetakan adalah putih bersih */
            html, body {
                background-color: #ffffff !important;
                background-image: none !important;
                color: #000000 !important;
                width: 100% !important;
                height: 100% !important;
                margin: 0 !important;
                padding: 0 !important;
                overflow: visible !important;
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }

            /* Matikan pembungkus fleksibel / gelap semasa cetak */
            main, .canvas-container {
                background: none !important;
                padding: 0 !important;
                margin: 0 !important;
                overflow: visible !important;
                display: block !important;
                position: absolute !important;
                top: 0 !important;
                left: 0 !important;
                width: 100% !important;
                height: 100% !important;
            }

            #canvas-wrapper {
                transform: none !important; /* Paksa skala cetak 100% fisik */
                position: absolute !important;
                top: 0 !important;
                left: 0 !important;
                margin: 0 !important;
                padding: 0 !important;
            }

            #print-area {
                position: absolute !important;
                top: 0 !important;
                left: 0 !important;
                margin: 0 !important;
                padding: 0 !important;
                border: none !important;
                box-shadow: none !important;
                background: #ffffff !important;
                page-break-inside: avoid;
            }

            /* Jamin area dropzone terposisi tepat dengan margin saat print */
            #photo-dropzone {
                position: absolute !important;
            }
        }

        /* Kelas Reka Bentuk UI Photoshop */
        .ps-panel {
            background-color: #252526;
            border: 1px solid #3c3c3c;
        }
        .ps-input {
            background-color: #3c3c3c;
            border: 1px solid #555555;
            color: #f3f3f3;
        }
        .ps-input:focus {
            border-color: #007acc;
            outline: none;
        }
        .canvas-container {
            background-color: #1e1e1e;
            background-image: radial-gradient(#2d2d2d 1px, transparent 1px);
            background-size: 16px 16px;
        }
        
        /* Pembungkus Elemen Aktif (Foto & Bentuk & Teks) */
        .photo-wrapper {
            position: absolute;
            cursor: move;
            touch-action: none;
        }
        .photo-wrapper.selected {
            border: 2px dashed #007acc;
        }

        /* Mode Crop/Edit/Lengkung Aktif (Garis Amber Menyala Khas Photoshop) */
        .photo-wrapper.crop-mode-active {
            border: 2px dashed #f59e0b !important;
            box-shadow: 0 0 12px rgba(245, 158, 11, 0.6) !important;
        }
        
        /* Gaya Premium untuk 8 Titik Kontrol Ukuran (Resize Handles) */
        .resize-handle {
            position: absolute;
            width: 8px;
            height: 8px;
            background-color: #ffffff;
            border: 1.5px solid #007acc;
            display: none;
            z-index: 40;
        }
        .photo-wrapper.selected .resize-handle {
            display: block;
        }

        /* Gaya Premium untuk Draggable Vector Nodes (Edit Titik Shapes) */
        .node-handle {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #f59e0b; /* Amber */
            border: 1.5px solid #ffffff;
            border-radius: 50%;
            transform: translate(-50%, -50%);
            z-index: 50;
            cursor: pointer;
            box-shadow: 0 0 4px rgba(0,0,0,0.5);
        }
        .node-handle:hover {
            background-color: #d97706; /* Darker Amber */
            transform: translate(-50%, -50%) scale(1.3);
        }

        /* Area Teks di Inside Shapes & Text Layer */
        .shape-text-overlay {
            outline: none;
            word-wrap: break-word;
            word-break: break-all;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.15s ease;
        }
    </style>
</head>
<body class="h-screen flex flex-col select-none">

    <!-- BAR MENU ATAS (Logo & Merek FM Collection) -->
    <header class="h-10 ps-panel border-b flex items-center justify-between px-4 text-xs z-50 no-print">
        <div class="flex items-center space-x-4">
            <span class="font-bold text-sky-400 flex items-center gap-2">
                <!-- Tampilan Logo Utama FM Collection -->
                <img src="https://iili.io/CBfHoR1.png" alt="FM Collection Logo" class="h-6 w-auto object-contain rounded-sm" onerror="this.src='https://via.placeholder.com/24/007acc/ffffff?text=FM'">
                <span>FM COLLECTION</span>
            </span>
            <div class="hidden md:flex space-x-3 text-gray-400">
                <button class="hover:text-white transition" onclick="resetCanvas()">Fail Baru</button>
                <button class="hover:text-white transition" onclick="triggerUpload()">Import Foto</button>
                <button class="hover:text-white transition" onclick="openMarginModal()">Atur Margin</button>
                <button class="hover:text-white transition" onclick="printCanvas()">Cetak (Ctrl+P)</button>
                <button class="hover:text-white transition" onclick="clearCanvas()">Kosongkan Kanvas</button>
            </div>
        </div>
        <div class="text-gray-400 text-[11px] flex items-center gap-2">
            <span class="px-2 py-0.5 bg-zinc-800 rounded text-zinc-400">Layout Presisi 96 DPI</span>
            <span class="text-zinc-500">v3.9.5 (Word Shapes Fixed)</span>
        </div>
    </header>

    <!-- RUANG KERJA UTAMA -->
    <div class="flex-1 flex overflow-hidden relative">
        
        <!-- TOOLBAR SISI (Kiri) -->
        <aside class="w-14 ps-panel border-r flex flex-col items-center py-4 space-y-4 no-print z-40">
            <!-- Alat Unggah Gambar -->
            <button onclick="triggerUpload()" class="w-10 h-10 rounded-lg flex items-center justify-center text-gray-300 hover:bg-[#37373d] hover:text-white transition" title="Import Foto Baru">
                <i class="fa-solid fa-file-import text-lg"></i>
            </button>
            <!-- Pemisah -->
            <div class="w-8 h-[1px] bg-zinc-700"></div>
            <!-- Alat Pemilihan & Gerak -->
            <button class="w-10 h-10 rounded-lg flex items-center justify-center text-sky-400 bg-[#2d2d2d] transition" title="Alat Menggerak & Saiz">
                <i class="fa-solid fa-arrows-up-down-left-right text-lg"></i>
            </button>
            <!-- Alat Teks Bebas (Text Tool) -->
            <button onclick="createNewTextElement()" class="w-10 h-10 rounded-lg flex items-center justify-center text-yellow-400 hover:bg-[#37373d] hover:text-white transition" title="Tambah Teks Bebas Mandiri">
                <i class="fa-solid fa-font text-lg"></i>
            </button>
            <!-- Alat Margin Cepat -->
            <button onclick="openMarginModal()" class="w-10 h-10 rounded-lg flex items-center justify-center text-gray-300 hover:bg-[#37373d] hover:text-white transition" title="Pengaturan Margin Kertas">
                <i class="fa-solid fa-border-top-left text-lg"></i>
            </button>
            <!-- Susunan Grid Automatik -->
            <button onclick="autoLayoutPhotos()" class="w-10 h-10 rounded-lg flex items-center justify-center text-gray-300 hover:bg-[#37373d] hover:text-white transition" title="Susun Automatik (Grid)">
                <i class="fa-solid fa-table-cells text-lg"></i>
            </button>
            <!-- Alat Putar -->
            <button onclick="rotateSelected(90)" class="w-10 h-10 rounded-lg flex items-center justify-center text-gray-300 hover:bg-[#37373d] hover:text-white transition" title="Putar Kanan 90°">
                <i class="fa-solid fa-rotate text-lg"></i>
            </button>
            <!-- Duplikasi Objek -->
            <button onclick="duplicateSelected()" class="w-10 h-10 rounded-lg flex items-center justify-center text-gray-300 hover:bg-[#37373d] hover:text-white transition" title="Duplikasi Objek Terpilih">
                <i class="fa-solid fa-copy text-lg"></i>
            </button>
            <!-- Padam Objek -->
            <button onclick="deleteSelected()" class="w-10 h-10 rounded-lg flex items-center justify-center text-red-400 hover:bg-red-950/30 transition" title="Padam Objek Terpilih">
                <i class="fa-solid fa-trash-can text-lg"></i>
            </button>
            
            <div class="flex-1"></div>
            
            <!-- Kawalan Zum Kanvas -->
            <button onclick="zoomCanvas(0.1)" class="w-8 h-8 rounded flex items-center justify-center text-gray-400 hover:bg-zinc-800" title="Zum Masuk">
                <i class="fa-solid fa-magnifying-glass-plus"></i>
            </button>
            <button onclick="zoomCanvas(-0.1)" class="w-8 h-8 rounded flex items-center justify-center text-gray-400 hover:bg-zinc-800" title="Zum Keluar">
                <i class="fa-solid fa-magnifying-glass-minus"></i>
            </button>
            <span id="zoom-label" class="text-[10px] text-zinc-500 font-bold">100%</span>
        </aside>

        <!-- KANVAS STUDIO (Tengah) -->
        <main class="flex-1 canvas-container overflow-auto p-8 flex justify-center items-start relative">
            <div id="canvas-wrapper" class="origin-top transition-transform duration-100 ease-out">
                <!-- Kawasan Kertas Fizikal -->
                <div id="print-area" class="bg-white shadow-2xl relative border border-gray-400 transition-all duration-300" style="width: 794px; height: 1123px;">
                    <!-- Penunjuk Maklumat Kertas Sisi Layar -->
                    <div class="absolute top-2 left-2 text-[10px] text-gray-400 pointer-events-none no-print uppercase font-mono tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-circle text-[6px] text-emerald-500 animate-pulse"></i>
                        Kanvas Kerja: <span id="canvas-info-display">A4 (210 x 297 mm)</span>
                    </div>

                    <!-- PENUNJUK GARIS PANDUAN MARGIN (Hanya Muncul di Editor) -->
                    <div id="margin-guide" class="absolute border border-dashed border-rose-400/60 pointer-events-none z-10 no-print transition-all duration-300">
                        <div class="absolute -top-4 left-1/2 -translate-x-1/2 text-[8px] bg-rose-500 text-white px-1 rounded font-mono shadow">Batas Margin Aman</div>
                    </div>
                    
                    <!-- KAWASAN MELETAKKAN GAMBAR & BENTUK & TEKS -->
                    <div id="photo-dropzone" class="absolute transition-all duration-300">
                        <!-- Objek dijana secara dinamik di sini -->
                    </div>
                </div>
            </div>

            <!-- Input Fail Tersembunyi -->
            <input type="file" id="image-upload" accept="image/*" class="hidden" multiple onchange="handleImageUpload(event)">
        </main>

        <!-- PANEL KAWALAN (Kanan) -->
        <aside class="w-80 ps-panel border-l flex flex-col no-print z-40 overflow-y-auto">
            
            <!-- SEKSI 1: PENGATURAN KERTAS & MARGIN -->
            <div class="p-4 border-b border-zinc-700">
                <h3 class="text-xs font-semibold text-zinc-400 uppercase tracking-wider mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-scroll text-sky-400"></i> 1. Pengaturan Kertas
                </h3>
                <div class="space-y-3">
                    <div>
                        <label class="block text-[11px] text-zinc-400 mb-1">Pilih Saiz Kertas</label>
                        <select id="canvas-size" onchange="changeCanvasSize()" class="w-full ps-input rounded p-2 text-xs">
                            <option value="A4" selected>A4 (21 x 29.7 cm)</option>
                            <option value="F4">F4 / Folio (21.5 x 33 cm)</option>
                            <option value="A3">A3 (29.7 x 42 cm)</option>
                            <option value="CUSTOM">📐 Kertas Custom (Kustom)</option>
                        </select>
                    </div>

                    <!-- BLOK INPUT DIMENSI KERTAS CUSTOM (Hanya Tampil Jika Kertas Custom Terpilih) -->
                    <div id="custom-paper-inputs" class="hidden space-y-2 p-2.5 bg-zinc-900/60 rounded border border-zinc-800">
                        <span class="text-[9.5px] text-amber-400 font-bold block uppercase tracking-wider"><i class="fa-solid fa-sliders"></i> Dimensi Kertas Kustom</span>
                        <div class="grid grid-cols-3 gap-1.5">
                            <div>
                                <label class="block text-[9px] text-zinc-500 mb-0.5">Lebar</label>
                                <input type="number" id="custom-paper-w" value="30.0" step="0.1" min="5" class="w-full ps-input rounded p-1 text-xs text-center font-bold" oninput="applyCustomPaperSize()">
                            </div>
                            <div>
                                <label class="block text-[9px] text-zinc-500 mb-0.5">Tinggi</label>
                                <input type="number" id="custom-paper-h" value="30.0" step="0.1" min="5" class="w-full ps-input rounded p-1 text-xs text-center font-bold" oninput="applyCustomPaperSize()">
                            </div>
                            <div>
                                <label class="block text-[9px] text-zinc-500 mb-0.5">Unit</label>
                                <select id="custom-paper-unit" class="w-full ps-input rounded p-1 text-[10px] h-[26px]" onchange="applyCustomPaperSize()">
                                    <option value="cm" selected>Cm</option>
                                    <option value="inch">Inch</option>
                                    <option value="mm">Mm</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div>
                        <label class="block text-[11px] text-zinc-400 mb-1">Orientasi Kertas</label>
                        <div class="grid grid-cols-2 gap-2">
                            <button id="orient-portrait" onclick="changeOrientation('portrait')" class="py-1.5 px-3 rounded text-xs font-medium ps-panel border border-sky-500 text-sky-400 flex items-center justify-center gap-1">
                                <i class="fa-solid fa-file-portrait"></i> Tegak
                            </button>
                            <button id="orient-landscape" onclick="changeOrientation('landscape')" class="py-1.5 px-3 rounded text-xs font-medium ps-panel border border-zinc-700 hover:border-zinc-500 flex items-center justify-center gap-1">
                                <i class="fa-solid fa-file-landscape"></i> Mendatar
                            </button>
                        </div>
                    </div>
                    
                    <!-- TOMBOL ATUR MARGIN KERTAS -->
                    <button onclick="openMarginModal()" class="w-full py-2 bg-zinc-800 hover:bg-zinc-700 border border-zinc-600 rounded text-xs font-medium text-zinc-300 flex items-center justify-center gap-1.5 transition">
                        <i class="fa-solid fa-arrows-to-dot text-amber-400"></i> Atur Margin Kertas Cetak
                    </button>
                    <!-- Tampilan Margin Atas -->
                    <div class="text-[10px] text-zinc-500 font-mono text-center bg-zinc-900/50 p-1 rounded">
                        Margin: <span id="lbl-margin-top">1.0</span>cm (A) | <span id="lbl-margin-bottom">1.0</span>cm (B) | <span id="lbl-margin-left">1.0</span>cm (K) | <span id="lbl-margin-right">1.0</span>cm (Kn)
                    </div>
                </div>
            </div>

            <!-- SEKSI 2: PRESET UKURAN OTOMATIS -->
            <div class="p-4 border-b border-zinc-700">
                <h3 class="text-xs font-semibold text-zinc-400 uppercase tracking-wider mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-crop-simple text-sky-400"></i> 2. Saiz Foto Automatik
                </h3>
                
                <div class="grid grid-cols-3 gap-1 mb-3 text-[10px] bg-zinc-900 p-1 rounded">
                    <button onclick="switchTab('tab-pasfoto')" id="btn-tab-pasfoto" class="py-1 rounded bg-zinc-700 text-white font-medium text-center">Pasfoto</button>
                    <button onclick="switchTab('tab-r')" id="btn-tab-r" class="py-1 rounded text-zinc-400 hover:text-white text-center">Ukuran R</button>
                    <button onclick="switchTab('tab-card')" id="btn-tab-card" class="py-1 rounded text-zinc-400 hover:text-white text-center">Kad</button>
                </div>

                <!-- PANEL PAS FOTO -->
                <div id="tab-pasfoto" class="tab-content space-y-2">
                    <div class="grid grid-cols-3 gap-1">
                        <button onclick="setPhotoPreset(2, 3, 'Pasfoto 2x3')" class="py-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">2 x 3</span>
                            <span class="text-[9px] text-zinc-400">2.1 x 2.7 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(3, 4, 'Pasfoto 3x4')" class="py-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">3 x 4</span>
                            <span class="text-[9px] text-zinc-400">2.8 x 3.8 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(4, 6, 'Pasfoto 4x6')" class="py-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">4 x 6</span>
                            <span class="text-[9px] text-zinc-400">3.8 x 5.6 cm</span>
                        </button>
                    </div>
                </div>

                <!-- PANEL UKURAN R -->
                <div id="tab-r" class="tab-content hidden space-y-2">
                    <div class="grid grid-cols-3 gap-1">
                        <button onclick="setPhotoPreset(6, 9, 'Foto 2R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">2R</span>
                            <span class="text-[9px] text-zinc-400">6 x 9 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(8.9, 12.7, 'Foto 3R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">3R</span>
                            <span class="text-[9px] text-zinc-400">8.9x12.7 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(10.2, 15.2, 'Foto 4R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">4R</span>
                            <span class="text-[9px] text-zinc-400">10.2x15.2 cm</span>
                        </button>
                    </div>
                    <div class="grid grid-cols-3 gap-1 mt-1">
                        <button onclick="setPhotoPreset(12.7, 17.8, 'Foto 5R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">5R</span>
                            <span class="text-[9px] text-zinc-400">12.7x17.8 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(15.2, 20.3, 'Foto 6R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">6R</span>
                            <span class="text-[9px] text-zinc-400">15.2x20.3 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(20.3, 25.4, 'Foto 8R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">8R</span>
                            <span class="text-[9px] text-zinc-400">20.3x25.4 cm</span>
                        </button>
                    </div>
                    <div class="grid grid-cols-3 gap-1 mt-1">
                        <button onclick="setPhotoPreset(25.4, 30.5, 'Foto 10R')" class="p-1 bg-zinc-800 hover:bg-zinc-700 text-[11px] text-center rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold">10R</span>
                            <span class="text-[9px] text-zinc-400">25.4x30.5 cm</span>
                        </button>
                    </div>
                </div>

                <!-- PANEL UKURAN KAD -->
                <div id="tab-card" class="tab-content hidden space-y-2">
                    <div class="grid grid-cols-2 gap-1">
                        <button onclick="setPhotoPreset(8.56, 5.398, 'KTP / ID Card')" class="py-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold text-[11px]">KTP / SIM</span>
                            <span class="text-[9px] text-zinc-400">8.56 x 5.4 cm</span>
                        </button>
                        <button onclick="setPhotoPreset(9, 5.5, 'Kartu Nama')" class="py-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-700 flex flex-col items-center">
                            <span class="font-bold text-[11px]">Kartu Nama</span>
                            <span class="text-[9px] text-zinc-400">9.0 x 5.5 cm</span>
                        </button>
                    </div>
                </div>

                <div class="mt-3 p-2 bg-zinc-900 rounded border border-zinc-800 text-[11px]">
                    <span class="text-zinc-400 font-medium">Dimensi Tangguh: </span>
                    <span id="selected-preset-label" class="font-bold text-sky-400">Pasfoto 3x4 (2.8 x 3.8 cm)</span>
                </div>
            </div>

            <!-- SEKSI 3: UKURAN CUSTOM -->
            <div class="p-4 border-b border-zinc-700">
                <h3 class="text-xs font-semibold text-zinc-400 uppercase tracking-wider mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-sky-400"></i> 3. Saiz Kustom (Custom)
                </h3>
                <div class="grid grid-cols-3 gap-2 mb-3">
                    <div class="col-span-1">
                        <label class="block text-[10px] text-zinc-400 mb-0.5">Lebar</label>
                        <input type="number" id="custom-width" value="10" step="0.1" class="w-full ps-input rounded p-1 text-xs text-center" oninput="updateCustomSelection()">
                    </div>
                    <div class="col-span-1">
                        <label class="block text-[10px] text-zinc-400 mb-0.5">Tinggi</label>
                        <input type="number" id="custom-height" value="15" step="0.1" class="w-full ps-input rounded p-1 text-xs text-center" oninput="updateCustomSelection()">
                    </div>
                    <div class="col-span-1">
                        <label class="block text-[10px] text-zinc-400 mb-0.5">Unit</label>
                        <select id="custom-unit" class="w-full ps-input rounded p-1 text-[11px]" onchange="updateCustomSelection()">
                            <option value="cm" selected>Cm</option>
                            <option value="inch">Inch</option>
                            <option value="mm">Mm</option>
                        </select>
                    </div>
                </div>
                <div class="p-4 border-b border-zinc-700">
                    <button onclick="applyCustomSize()" class="w-full py-1.5 bg-sky-600 hover:bg-sky-500 text-white rounded text-xs font-medium transition flex items-center justify-center gap-1">
                        <i class="fa-solid fa-arrows-left-right-to-line"></i> Terapkan Saiz Kustom
                    </button>
                </div>
            </div>

            <!-- SEKSI 4: SHAPE TOOL TERKINI (KATEGORI MICROSOFT WORD) -->
            <div class="p-4 border-b border-zinc-700 bg-[#2d2d30]">
                <h3 class="text-xs font-semibold text-amber-400 uppercase tracking-wider mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-shapes"></i> 4. Tambah Bentuk (Shape Tool)
                </h3>
                
                <!-- Tab Kategori Bentuk -->
                <div class="space-y-3">
                    <!-- Kelompok 1: Bentuk Standar -->
                    <div>
                        <span class="text-[9px] text-zinc-500 uppercase font-bold tracking-wider block mb-1">Bentuk Asas</span>
                        <div class="grid grid-cols-7 gap-1">
                            <button onclick="createShapeElement('rect')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Segi Empat"><i class="fa-regular fa-square"></i></button>
                            <button onclick="createShapeElement('circle')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Bulatan"><i class="fa-regular fa-circle"></i></button>
                            <button onclick="createShapeElement('triangle')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Segi Tiga"><i class="fa-solid fa-caret-up"></i></button>
                            <button onclick="createShapeElement('line')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs text-amber-400 font-bold" title="Garis Lurus"><i class="fa-solid fa-minus"></i></button>
                            <button onclick="createShapeElement('pentagon')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Segi Lima"><i class="fa-solid fa-dice-d20"></i></button>
                            <button onclick="createShapeElement('hexagon')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Segi Enam"><span class="font-bold text-[10px]">H6</span></button>
                            <button onclick="createShapeElement('octagon')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Segi Lapan"><span class="font-bold text-[10px]">O8</span></button>
                        </div>
                    </div>

                    <!-- Kelompok 2: Anak Panah (Arrows) -->
                    <div>
                        <span class="text-[9px] text-zinc-500 uppercase font-bold tracking-wider block mb-1">Anak Panah</span>
                        <div class="grid grid-cols-5 gap-1">
                            <button onclick="createShapeElement('arrow-right')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Panah Kanan"><i class="fa-solid fa-arrow-right"></i></button>
                            <button onclick="createShapeElement('arrow-left')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Panah Kiri"><i class="fa-solid fa-arrow-left"></i></button>
                            <button onclick="createShapeElement('arrow-up')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Panah Atas"><i class="fa-solid fa-arrow-up"></i></button>
                            <button onclick="createShapeElement('arrow-down')" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-center text-xs" title="Panah Bawah"><i class="fa-solid fa-arrow-down"></i></button>
                            <button onclick="createShapeElement('arrow-4way')" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-center text-xs" title="Panah 4 Arah"><i class="fa-solid fa-arrows-alt"></i></button>
                        </div>
                    </div>

                    <!-- Kelompok 3: Bintang & Banner -->
                    <div>
                        <span class="text-[9px] text-zinc-500 uppercase font-bold tracking-wider block mb-1">Bintang & Hiasan</span>
                        <div class="grid grid-cols-6 gap-1">
                            <button onclick="createShapeElement('star')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Bintang 5 Sudut"><i class="fa-regular fa-star"></i></button>
                            <button onclick="createShapeElement('star-4')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-[10px]" title="Bintang 4 Sudut"><span class="font-bold">★4</span></button>
                            <button onclick="createShapeElement('star-6')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-[10px]" title="Bintang 6 Sudut"><span class="font-bold">★6</span></button>
                            <button onclick="createShapeElement('heart')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs text-rose-500" title="Hati / Love"><i class="fa-solid fa-heart"></i></button>
                            <button onclick="createShapeElement('speech-bubble')" class="py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-center text-xs" title="Gelembung Bicara"><i class="fa-regular fa-comment"></i></button>
                            <button onclick="createShapeElement('banner-ribbon')" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-center text-xs text-amber-500" title="Pita Banner"><i class="fa-solid fa-bookmark"></i></button>
                        </div>
                    </div>
                </div>

                <div class="text-[9.5px] text-zinc-400 mt-2.5 text-center leading-normal">
                    Klik bentuk untuk tambah. <b>Klik dua kali</b> bentuk pada kanvas untuk mengedit teks di dalamnya!
                </div>
            </div>

            <!-- SEKSI 5: PENGATURAN OBJEK TERPILIH (DINAMIK) -->
            <div class="p-4 border-b border-zinc-700 bg-[#252526] flex-1">
                <h3 class="text-xs font-semibold text-zinc-300 uppercase tracking-wider mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-emerald-400"></i> Pengaturan Objek Terpilih
                </h3>

                <!-- Mesej Tiada Objek Terpilih -->
                <div id="no-photo-selected-msg" class="text-zinc-500 text-center py-6 text-xs border border-dashed border-zinc-700 rounded">
                    Sila klik mana-mana gambar, bentuk geometri, atau lapisan teks pada kanvas kerja untuk mula menyunting.
                </div>

                <!-- Panel Kawalan Aktif -->
                <div id="photo-selected-controls" class="hidden space-y-4">
                    
                    <!-- KAWALAN PAN / CROP KHAS FOTO -->
                    <div id="photo-crop-control" class="space-y-2 p-2 bg-zinc-900/60 rounded border border-zinc-800">
                        <span class="text-sky-400 font-bold text-[11px] block uppercase tracking-wider mb-1">
                            <i class="fa-solid fa-crop"></i> Penyesuaian Potongan (Crop)
                        </span>
                        
                        <div class="grid grid-cols-2 gap-1 mb-2">
                            <button id="btn-mode-move-frame" onclick="setDragMode('frame')" class="py-1 px-2 text-[10px] bg-sky-600 rounded text-white font-medium border border-sky-500 flex items-center justify-center gap-1" title="Menggeser letak foto pada kanvas">
                                <i class="fa-solid fa-arrows-up-down-left-right"></i> Geser Bingkai
                            </button>
                            <button id="btn-mode-move-content" onclick="setDragMode('content')" class="py-1 px-2 text-[10px] bg-zinc-800 hover:bg-zinc-700 rounded text-zinc-300 border border-zinc-700 flex items-center justify-center gap-1" title="Menyeret foto ke dalam bingkai agar tidak terpotong">
                                <i class="fa-solid fa-hand"></i> Geser Isi Foto
                            </button>
                        </div>
                        
                        <div>
                            <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                <span>Posisi Vertikal (Y)</span>
                                <span id="crop-y-val" class="text-zinc-300 font-semibold">50%</span>
                            </label>
                            <input type="range" id="photo-crop-y" min="0" max="100" value="50" class="w-full accent-sky-500" oninput="updateCropProperties()">
                        </div>
                        
                        <div>
                            <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                <span>Posisi Horisontal (X)</span>
                                <span id="crop-x-val" class="text-zinc-300 font-semibold">50%</span>
                            </label>
                            <input type="range" id="photo-crop-x" min="0" max="100" value="50" class="w-full accent-sky-500" oninput="updateCropProperties()">
                        </div>
                        
                        <p class="text-[9.5px] text-amber-400 italic mt-1 leading-normal">
                            💡 <b>Pintasan Khas Photoshop:</b> Klik ganda (Double Click) foto di atas untuk Masuk/Keluar mode Crop secara cepat!
                        </p>
                    </div>

                    <!-- KAWALAN WARNA & TEKS KHAS SHAPES/BENTUK (TERKINI & LENGKAP) -->
                    <div id="shape-colors-control" class="hidden space-y-3 p-2 bg-zinc-900/60 rounded border border-zinc-800">
                        <span class="text-amber-400 font-bold text-[11px] block uppercase tracking-wider mb-1"><i class="fa-solid fa-palette"></i> Panel Sunting Teks & Bentuk</span>
                        
                        <!-- 1. Kustomisasi Warna Vektor (Akan Disembunyikan Otomatis untuk Teks Murni) -->
                        <div id="shape-fill-stroke-inputs" class="grid grid-cols-2 gap-2 text-xs">
                            <div>
                                <label class="block text-zinc-400 text-[10px] mb-0.5">Warna Isi (Fill)</label>
                                <input type="color" id="shape-fill-color" class="w-full h-8 rounded bg-zinc-800 border border-zinc-700 cursor-pointer" oninput="updateShapeProperties()">
                            </div>
                            <div>
                                <label class="block text-zinc-400 text-[10px] mb-0.5">Sempadan (Stroke)</label>
                                <input type="color" id="shape-stroke-color" class="w-full h-8 rounded bg-zinc-800 border border-zinc-700 cursor-pointer" oninput="updateShapeProperties()">
                            </div>
                        </div>
                        
                        <!-- KELENGKUNGAN SUDUT / CORNER ROUNDING SLIDER -->
                        <div class="space-y-1">
                            <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                <span>Kelengkungan Sudut Shapes</span>
                                <span id="corner-radius-val" class="text-zinc-300 font-semibold">0px</span>
                            </label>
                            <input type="range" id="shape-corner-radius" min="0" max="50" value="0" class="w-full accent-emerald-500" oninput="updateShapeCornerRadius()">
                        </div>

                        <!-- TOMBOL KONTROL EDIT VERTEKS/NODE SHAPES -->
                        <div class="pt-1">
                            <button id="btn-toggle-node-edit" onclick="toggleNodeEditMode()" class="w-full py-1.5 bg-zinc-800 hover:bg-zinc-700 text-amber-400 rounded text-xs font-bold border border-zinc-700 transition flex items-center justify-center gap-1.5">
                                <i class="fa-solid fa-bezier-curve"></i> Aktifkan Edit Titik (Pen Tool)
                            </button>
                        </div>

                        <div id="shape-stroke-width-container">
                            <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                <span>Ketebalan Sempadan</span>
                                <span id="stroke-val" class="text-zinc-300 font-semibold">2px</span>
                            </label>
                            <input type="range" id="shape-stroke-width" min="0" max="20" value="2" class="w-full accent-emerald-500" oninput="updateShapeProperties()">
                        </div>

                        <!-- 2. Kustomisasi Gaya Teks Shapes & Text Layer -->
                        <div class="pt-2 border-t border-zinc-700 space-y-2">
                            <span class="text-sky-400 font-bold text-[10px] block uppercase tracking-wider"><i class="fa-solid fa-font"></i> Gaya & Atribut Teks</span>
                            
                            <!-- Keluarga Font Dropdown (Premium & Melimpah) -->
                            <div>
                                <label class="block text-zinc-400 text-[10px] mb-1">Keluarga Font (Font Family)</label>
                                <select id="shape-font-family" class="w-full ps-input rounded p-1.5 text-xs font-semibold text-zinc-100" onchange="updateShapeTextProperties()">
                                    <!-- Sans Serif -->
                                    <optgroup label="Sans-Serif (Modern)">
                                        <option value="'Inter', sans-serif">Inter (Bersih & Rapi)</option>
                                        <option value="'Montserrat', sans-serif">Montserrat (Geometrik Premium)</option>
                                        <option value="'Roboto', sans-serif">Roboto (Sederhana)</option>
                                        <option value="Arial, sans-serif">Arial (Standard)</option>
                                        <option value="'Fredoka', sans-serif">Fredoka (Bulat & Lucu)</option>
                                    </optgroup>
                                    <!-- Serif -->
                                    <optgroup label="Serif (Klasik/Resmi)">
                                        <option value="'Playfair Display', serif">Playfair Display (Elegan / Logo)</option>
                                        <option value="'Lora', serif">Lora (Gaya Seni)</option>
                                        <option value="'Times New Roman', serif">Times New Roman (Koran Resmi)</option>
                                        <option value="Georgia, serif">Georgia (Sastra)</option>
                                    </optgroup>
                                    <!-- Script & Decorative -->
                                    <optgroup label="Artistik & Tulis Tangan">
                                        <option value="'Pacifico', cursive">Pacifico (Tulis Tangan Estetis)</option>
                                        <option value="'Oswald', sans-serif">Oswald (Tebal & Ramping)</option>
                                        <option value="'Lobster', cursive">Lobster (Modern Bold Script)</option>
                                        <option value="'Caveat', cursive">Caveat (Sketsa Tangan Bebas)</option>
                                        <option value="Impact, Charcoal, sans-serif">Impact (Judul Sangat Tebal)</option>
                                        <option value="'Courier New', monospace">Courier New (Retro Mesin Tik)</option>
                                    </optgroup>
                                </select>
                            </div>

                            <div class="grid grid-cols-2 gap-2 text-xs">
                                <div>
                                    <label class="block text-zinc-400 text-[10px] mb-0.5">Warna Teks</label>
                                    <input type="color" id="shape-text-color" class="w-full h-7 rounded bg-zinc-800 border border-zinc-700 cursor-pointer" oninput="updateShapeTextProperties()">
                                </div>
                                <div>
                                    <label class="block text-zinc-400 text-[10px] mb-0.5">Ukuran Teks</label>
                                    <div class="relative">
                                        <input type="number" id="shape-text-size" value="24" min="6" max="150" class="w-full ps-input rounded p-1 text-xs text-center" oninput="updateShapeTextProperties()">
                                        <span class="absolute right-1.5 top-1 text-[9px] text-zinc-500">px</span>
                                    </div>
                                </div>
                            </div>

                            <!-- KELENGKUNGAN TEKS (WARP TEXT / CURVE SLIDER) -->
                            <div class="space-y-1">
                                <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                    <span>Kelengkungan Teks (Warp)</span>
                                    <span id="text-curve-val" class="text-amber-400 font-bold">0</span>
                                </label>
                                <input type="range" id="shape-text-curve" min="-100" max="100" value="0" class="w-full accent-amber-500" oninput="updateShapeTextProperties()">
                            </div>

                            <!-- SPASI KARAKTER (LETTER SPACING) -->
                            <div class="space-y-1">
                                <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                    <span>Jarak Huruf (Spacing)</span>
                                    <span id="text-spacing-val" class="text-zinc-300 font-semibold">0px</span>
                                </label>
                                <input type="range" id="shape-text-spacing" min="-2" max="25" value="0" class="w-full accent-sky-500" oninput="updateShapeTextProperties()">
                            </div>

                            <!-- JARAK BARIS (LINE HEIGHT) -->
                            <div class="space-y-1">
                                <label class="block text-zinc-400 text-[10px] mb-0.5 flex justify-between">
                                    <span>Jarak Baris (Line Height)</span>
                                    <span id="text-lineheight-val" class="text-zinc-300 font-semibold">1.2</span>
                                </label>
                                <input type="range" id="shape-text-lineheight" min="8" max="25" value="12" class="w-full accent-sky-500" oninput="updateShapeTextProperties()">
                            </div>

                            <!-- EFEK BAYANGAN TEKS (TEXT SHADOW) -->
                            <div class="space-y-2 p-1.5 bg-zinc-900/40 rounded border border-zinc-800">
                                <span class="text-emerald-400 font-bold text-[9px] block uppercase tracking-wider"><i class="fa-solid fa-circle-half-stroke"></i> Efek Bayangan (Shadow)</span>
                                <div class="grid grid-cols-2 gap-2 text-xs">
                                    <div>
                                        <label class="block text-zinc-400 text-[9px] mb-0.5">Warna Bayangan</label>
                                        <input type="color" id="shape-text-shadow-color" value="#000000" class="w-full h-7 rounded bg-zinc-800 border border-zinc-700 cursor-pointer" oninput="updateShapeTextProperties()">
                                    </div>
                                    <div>
                                        <label class="block text-zinc-400 text-[9px] mb-0.5 flex justify-between">
                                            <span>Keburaman</span>
                                        </label>
                                        <input type="range" id="shape-text-shadow-blur" min="0" max="15" value="0" class="w-full accent-emerald-500" oninput="updateShapeTextProperties()">
                                    </div>
                                </div>
                            </div>

                            <!-- Opsi Formatting & Alignment -->
                            <div class="space-y-1.5 pt-1">
                                <label class="block text-zinc-400 text-[10px]">Format & Perataan Teks</label>
                                <div class="flex space-x-1">
                                    <!-- Bold, Italic, Underline -->
                                    <button id="btn-text-bold" onclick="toggleTextFormat('bold')" class="flex-1 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs font-bold" title="Tebal (Bold)">B</button>
                                    <button id="btn-text-italic" onclick="toggleTextFormat('italic')" class="flex-1 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs italic" title="Miring (Italic)">I</button>
                                    <button id="btn-text-underline" onclick="toggleTextFormat('underline')" class="flex-1 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs underline" title="Garis Bawah (Underline)">U</button>
                                    
                                    <span class="text-zinc-600 px-1 self-center">|</span>
                                    
                                    <!-- Alignments -->
                                    <button id="btn-align-left" onclick="setTextAlignment('left')" class="px-2 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs" title="Rata Kiri"><i class="fa-solid fa-align-left"></i></button>
                                    <button id="btn-align-center" onclick="setTextAlignment('center')" class="px-2 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs" title="Rata Tengah"><i class="fa-solid fa-align-center"></i></button>
                                    <button id="btn-align-right" onclick="setTextAlignment('right')" class="px-2 py-1 bg-zinc-800 hover:bg-zinc-700 rounded border border-zinc-700 text-xs" title="Rata Kanan"><i class="fa-solid fa-align-right"></i></button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- KAWALAN BORDER KHAS FOTO -->
                    <div id="photo-border-control">
                        <span class="text-zinc-400 block text-xs mb-1">Border Cetak (Garis Potong)</span>
                        <div class="grid grid-cols-3 gap-1">
                            <button onclick="setSelectedBorder('none')" class="p-1 text-[10px] bg-zinc-800 rounded border border-zinc-700" id="border-btn-none">Tanpa Border</button>
                            <button onclick="setSelectedBorder('white')" class="p-1 text-[10px] bg-zinc-800 rounded border border-zinc-700" id="border-btn-white">Putih (Sisi)</button>
                            <button onclick="setSelectedBorder('black')" class="p-1 text-[10px] bg-zinc-800 rounded border border-zinc-700" id="border-btn-black">Hitam (Garis)</button>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <div>
                            <span class="text-zinc-400 block mb-1">Rotasi</span>
                            <div class="flex space-x-1">
                                <button onclick="rotateSelected(-90)" class="flex-1 bg-zinc-800 hover:bg-zinc-700 p-1 rounded border border-zinc-700 text-[10px]" title="Rotasi Kiri">
                                    <i class="fa-solid fa-rotate-left"></i> -90°
                                </button>
                                <button onclick="rotateSelected(90)" class="flex-1 bg-zinc-800 hover:bg-zinc-700 p-1 rounded border border-zinc-700 text-[10px]" title="Rotasi Kanan">
                                    <i class="fa-solid fa-rotate-right"></i> +90°
                                </button>
                            </div>
                        </div>
                        <div>
                            <span class="text-zinc-400 block mb-1">Mirroring</span>
                            <div class="flex space-x-1">
                                <button onclick="flipSelected('X')" class="flex-1 bg-zinc-800 hover:bg-zinc-700 p-1 rounded border border-zinc-700 text-[10px]" title="Balik Mendatar">
                                    <i class="fa-solid fa-arrows-left-right"></i> Flip H
                                </button>
                                <button onclick="flipSelected('Y')" class="flex-1 bg-zinc-800 hover:bg-zinc-700 p-1 rounded border border-zinc-700 text-[10px]" title="Balik Menegak">
                                    <i class="fa-solid fa-arrows-up-down"></i> Flip V
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- Kedudukan Pantas -->
                    <div>
                        <span class="text-zinc-400 block text-xs mb-1">Kedudukan Pantas di Kertas</span>
                        <div class="grid grid-cols-4 gap-1">
                            <button onclick="alignSelected('left')" class="p-1 bg-zinc-800 hover:bg-zinc-700 rounded text-xs" title="Rata Kiri"><i class="fa-solid fa-align-left"></i></button>
                            <button onclick="alignSelected('center')" class="p-1 bg-zinc-800 hover:bg-zinc-700 rounded text-xs" title="Rata Tengah Horisontal"><i class="fa-solid fa-align-center"></i></button>
                            <button onclick="alignSelected('right')" class="p-1 bg-zinc-800 hover:bg-zinc-700 rounded text-xs" title="Rata Kanan"><i class="fa-solid fa-align-right"></i></button>
                            <button onclick="alignSelected('top')" class="p-1 bg-zinc-800 hover:bg-zinc-700 rounded text-xs" title="Rata Atas"><i class="fa-solid fa-arrows-up-to-line"></i></button>
                        </div>
                    </div>

                    <div class="pt-2 flex gap-1">
                        <button onclick="duplicateSelected()" class="flex-1 py-1 px-2 bg-zinc-800 hover:bg-zinc-700 text-xs rounded border border-zinc-600 text-zinc-300">
                            <i class="fa-solid fa-copy mr-1"></i> Duplikat
                        </button>
                        <button onclick="deleteSelected()" class="flex-1 py-1 px-2 bg-red-950/40 hover:bg-red-900/50 text-xs rounded border border-red-800 text-red-200">
                            <i class="fa-solid fa-trash mr-1"></i> Padam
                        </button>
                    </div>
                </div>
            </div>

            <!-- SEKSI 6: AKSI UTAMA PRINT & EXPORT -->
            <div class="p-4 bg-zinc-900 border-t border-zinc-700 space-y-2 mt-auto">
                <button onclick="printCanvas()" class="w-full py-3 bg-emerald-600 hover:bg-emerald-500 text-white rounded font-bold text-sm tracking-wide transition shadow-lg flex items-center justify-center gap-2">
                    <i class="fa-solid fa-print"></i> CETAK KANVAS (PDF / PRINTER)
                </button>
                <div class="text-[10px] text-zinc-500 text-center flex flex-col gap-0.5">
                    <span>*Gunakan tetapan skala 100% (No Scale) semasa mencetak</span>
                    <span>untuk ketepatan saiz fizikal milimeter.</span>
                </div>
            </div>
        </aside>
    </div>

    <!-- JENDELA MODAL DIALOG MARGIN (BERGAYA DARK PHOTOSHOP) -->
    <div id="margin-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center hidden no-print">
        <div class="w-80 ps-panel rounded-lg shadow-2xl overflow-hidden border border-zinc-600">
            <!-- Header Modal -->
            <div class="bg-zinc-800 px-4 py-2.5 flex items-center justify-between border-b border-zinc-700 text-xs font-semibold text-zinc-200">
                <span class="flex items-center gap-1.5 text-amber-400">
                    <i class="fa-solid fa-arrows-to-dot"></i> Pengaturan Margin Kertas
                </span>
                <button onclick="closeMarginModal()" class="text-zinc-400 hover:text-white transition">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>
            <!-- Body Modal / Form Inputs -->
            <div class="p-4 space-y-4">
                <div class="text-[11px] text-zinc-400 leading-relaxed">
                    Tentukan ruang kosong tepi kertas cetak (dalam satuan <b>Sentimeter / cm</b>). Jaminan cetakan presisi bebas area potong printer.
                </div>
                
                <div class="grid grid-cols-2 gap-3 text-xs">
                    <div>
                        <label class="block text-zinc-400 text-[10px] mb-1 font-medium">Margin Atas (Top)</label>
                        <div class="relative">
                            <input type="number" id="m-input-top" step="0.1" min="0" value="1.0" class="w-full ps-input rounded p-1.5 text-center pr-6">
                            <span class="absolute right-2 top-1.5 text-[9px] text-zinc-500">cm</span>
                        </div>
                    </div>
                    <div>
                        <label class="block text-zinc-400 text-[10px] mb-1 font-medium">Margin Bawah (Bottom)</label>
                        <div class="relative">
                            <input type="number" id="m-input-bottom" step="0.1" min="0" value="1.0" class="w-full ps-input rounded p-1.5 text-center pr-6">
                            <span class="absolute right-2 top-1.5 text-[9px] text-zinc-500">cm</span>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3 text-xs">
                    <div>
                        <label class="block text-zinc-400 text-[10px] mb-1 font-medium">Margin Kiri (Left)</label>
                        <div class="relative">
                            <input type="number" id="m-input-left" step="0.1" min="0" value="1.0" class="w-full ps-input rounded p-1.5 text-center pr-6">
                            <span class="absolute right-2 top-1.5 text-[9px] text-zinc-500">cm</span>
                        </div>
                    </div>
                    <div>
                        <label class="block text-zinc-400 text-[10px] mb-1 font-medium">Margin Kanan (Right)</label>
                        <div class="relative">
                            <input type="number" id="m-input-right" step="0.1" min="0" value="1.0" class="w-full ps-input rounded p-1.5 text-center pr-6">
                            <span class="absolute right-2 top-1.5 text-[9px] text-zinc-500">cm</span>
                        </div>
                    </div>
                </div>

                <!-- Preset Margin Cepat -->
                <div class="pt-2 border-t border-zinc-700/60">
                    <span class="block text-[10px] text-zinc-400 mb-1.5 font-medium">Preset Margin Cepat:</span>
                    <div class="grid grid-cols-3 gap-1 text-[10px]">
                        <button onclick="setQuickMarginPreset(0.5)" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-zinc-300 rounded border border-zinc-700">Sempit (0.5cm)</button>
                        <button onclick="setQuickMarginPreset(1.0)" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-zinc-300 rounded border border-zinc-700">Normal (1.0cm)</button>
                        <button onclick="setQuickMarginPreset(2.0)" class="py-1 bg-zinc-800 hover:bg-zinc-700 text-zinc-300 rounded border border-zinc-700">Lebar (2.0cm)</button>
                    </div>
                </div>
            </div>
            <!-- Footer Modal -->
            <div class="bg-zinc-800 px-4 py-3 border-t border-zinc-700 flex justify-end space-x-2 text-xs">
                <button onclick="closeMarginModal()" class="px-3 py-1.5 bg-zinc-700 hover:bg-zinc-600 text-zinc-300 rounded transition font-medium">
                    Batal
                </button>
                <button onclick="applyMarginsFromForm()" class="px-4 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white rounded transition font-bold flex items-center gap-1">
                    <i class="fa-solid fa-circle-check"></i> Terapkan
                </button>
            </div>
        </div>
    </div>

    <!-- NOTIFIKASI TOAST INTERAKTIF -->
    <div id="toast-message" class="fixed bottom-4 left-1/2 transform -translate-x-1/2 bg-zinc-800 border border-zinc-700 px-4 py-2 rounded-lg text-xs shadow-xl hidden z-50 flex items-center gap-2">
        <i id="toast-icon" class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toast-text">Pemberitahuan</span>
    </div>

    <!-- UTAMA JAVASCRIPT APLIKASI -->
    <script>
        // === VARIABEL GLOBAL ===
        // Nisbah Ketepatan DPI: 1 Cm = 37.795 Piksel.
        const pxPerCm = 37.7952755906; 
        
        // Tetapan lalai kertas
        let currentCanvas = {
            size: 'A4',
            orientation: 'portrait',
            widthCm: 21.0,
            heightCm: 29.7
        };

        // Tetapan lalai margin kertas (dalam cm)
        let currentMargins = {
            top: 1.0,
            bottom: 1.0,
            left: 1.0,
            right: 1.0
        };

        // Saiz Preset Semasa
        let activePreset = {
            width: 3.0,
            height: 4.0,
            label: 'Pasfoto 3x4'
        };

        let zoomScale = 0.8;
        let photos = []; // Array semua elemen (foto, bentuk geometri, & teks)
        let selectedPhoto = null; // Menyimpan objek aktif terpilih
        let currentDragMode = 'frame'; // 'frame' untuk geser bingkai, 'content' untuk geser isi gambar
        
        // Pengurusan Kawalan Drag & Drop
        let isDragging = false;
        let isResizing = false;
        let isDraggingContent = false; // Mode geser isi gambar aktif
        let isEditingNodes = false; // Mode edit titik bentuk (Pen Tool)
        let draggedNodeHandle = null; // Titik kontrol vektor yang sedang ditarik

        let currentX, currentY;
        let initialX, initialY;
        let xOffset = 0, yOffset = 0;
        let resizeDirection = '';
        
        let initialWidth, initialHeight;
        let initialLeft, initialTop;
        let initialCropX = 50, initialCropY = 50;

        // Peta Dimensi Preset Standard
        const presets = {
            '2x3': { w: 2.1, h: 2.7, label: 'Pasfoto 2x3' },
            '3x4': { w: 2.8, h: 3.8, label: 'Pasfoto 3x4' },
            '4x6': { w: 3.8, h: 5.6, label: 'Pasfoto 4x6' },
            '2r': { w: 6.0, h: 9.0, label: 'Foto 2R' },
            '3r': { w: 8.9, h: 12.7, label: 'Foto 3R' },
            '4r': { w: 10.2, h: 15.2, label: 'Foto 4R' },
            '5r': { w: 12.7, h: 17.8, label: 'Foto 5R' },
            '6r': { w: 15.2, h: 20.3, label: 'Foto 6R' },
            '8r': { w: 20.3, h: 25.4, label: 'Foto 8R' },
            '10r': { w: 25.4, h: 30.5, label: 'Foto 10R' },
            'ktp': { w: 8.56, h: 5.398, label: 'KTP / ID Card' },
            'card': { w: 9.0, h: 5.5, label: 'Kartu Nama' }
        };

        // Peta Saiz Kertas Kerja
        const paperSizes = {
            'A4': { w: 21.0, h: 29.7, label: 'A4 (21 x 29.7 cm)' },
            'F4': { w: 21.5, h: 33.0, label: 'F4 / Folio (21.5 x 33 cm)' },
            'A3': { w: 29.7, h: 42.0, label: 'A3 (29.7 x 42 cm)' }
        };

        // === PELANCARAN PERTAMA ===
        window.addEventListener('load', () => {
            initApp();
            
            // Nyahpilih elemen sekiranya kawasan kosong kanvas diklik
            document.getElementById('photo-dropzone').addEventListener('mousedown', (e) => {
                if (e.target === document.getElementById('photo-dropzone')) {
                    deselectAllPhotos();
                }
            });

            // Pintasan Papan Kekunci
            window.addEventListener('keydown', (e) => {
                if ((e.key === 'Delete' || e.key === 'Backspace') && selectedPhoto) {
                    if (document.activeElement.tagName !== 'INPUT' && document.activeElement.tagName !== 'SELECT' && !document.activeElement.classList.contains('shape-text-overlay')) {
                        deleteSelected();
                    }
                }
                if ((e.ctrlKey || e.metaKey) && e.key === 'p') {
                    e.preventDefault();
                    printCanvas();
                }
            });
        });

        // === FUNGSI LOGIKA VECTOR NODE EDITOR & RENDERING ===
        function getShapePoints(el) {
            if (el.dataset.points) {
                return JSON.parse(el.dataset.points);
            }
            
            const type = el.dataset.shapeType;
            let points = [];
            
            if (type === 'rect') points = [[5,5], [95,5], [95,95], [5,95]];
            else if (type === 'circle') points = []; 
            else if (type === 'triangle') points = [[50,5], [95,92], [5,92]];
            else if (type === 'line') points = [[10, 50], [90, 50]]; // Garis lurus hanya menggunakan 2 titik
            else if (type === 'pentagon') points = [[50,5], [93,38], [77,88], [23,88], [7,38]];
            else if (type === 'hexagon') points = [[50,2], [92,27], [92,77], [50,98], [8,77], [8,27]];
            else if (type === 'octagon') points = [[29,5], [71,5], [95,29], [95,71], [71,95], [29,95], [5,71], [5,29]];
            else if (type === 'arrow-right') points = [[5,33], [55,33], [55,12], [95,50], [55,88], [55,67], [5,67]];
            else if (type === 'arrow-left') points = [[95,33], [45,33], [45,12], [5,50], [45,88], [45,67], [95,67]];
            else if (type === 'arrow-up') points = [[33,95], [33,45], [12,45], [50,5], [88,45], [67,45], [67,95]];
            else if (type === 'arrow-down') points = [[33,5], [33,55], [12,55], [50,95], [88,55], [67,55], [67,5]];
            else if (type === 'arrow-4way') points = [[50,2], [62,22], [54,22], [55,45], [75,45], [75,37], [95,50], [75,63], [75,55], [55,55], [54,78], [62,78], [50,98], [38,78], [46,78], [45,55], [25,55], [25,63], [5,50], [25,37], [25,45], [45,45], [46,22], [38,22]];
            else if (type === 'star') points = [[50,2], [64,36], [98,36], [70,57], [81,91], [50,70], [19,91], [30,57], [2,36], [36,36]];
            else if (type === 'star-4') points = [[50,2], [61,39], [98,50], [61,61], [50,98], [39,61], [2,50], [39,39]];
            else if (type === 'star-6') points = [[50,2], [63,28], [95,25], [75,50], [95,75], [63,72], [50,98], [37,72], [5,75], [25,50], [5,25], [37,28]];
            else if (type === 'heart') points = [[50,15], [65,2], [85,2], [98,15], [95,35], [75,60], [50,95], [25,60], [5,35], [2,15], [15,2], [35,2]];
            else if (type === 'speech-bubble') points = [[5,10], [95,10], [95,65], [55,65], [32,88], [32,65], [5,65]];
            else if (type === 'banner-ribbon') points = [[5,15], [95,15], [95,75], [50,60], [5,75]];
            
            el.dataset.points = JSON.stringify(points);
            return points;
        }

        function updateSVGPath(el) {
            const svg = el.querySelector('svg');
            if (!svg) return;

            const type = el.dataset.shapeType;
            const fill = el.dataset.fill || '#007acc';
            const stroke = el.dataset.stroke || '#ffffff';
            const strokeWidth = el.dataset.strokeWidth || '2';
            const strokeLinejoin = el.dataset.strokeLinejoin || 'miter';
            const rx = el.dataset.cornerRadius || '0';
            
            if (type === 'circle') {
                svg.innerHTML = `<circle cx="50" cy="50" r="44" fill="${fill}" stroke="${stroke}" stroke-width="${strokeWidth}" />`;
                return;
            }

            const points = getShapePoints(el);
            const pointsStr = points.map(pt => `${pt[0]},${pt[1]}`).join(' ');

            if (type === 'line') {
                // Menghubungkan 2 titik ujung pada garis lurus
                svg.innerHTML = `<line x1="${points[0][0]}" y1="${points[0][1]}" x2="${points[1][0]}" y2="${points[1][1]}" stroke="${fill}" stroke-width="${strokeWidth * 2 || 4}" stroke-linecap="round" />`;
            } else if (type === 'rect' && rx > 0 && el.dataset.pointsEdited !== 'true') {
                svg.innerHTML = `<rect x="5" y="5" width="90" height="90" rx="${rx}" ry="${rx}" fill="${fill}" stroke="${stroke}" stroke-width="${strokeWidth}" stroke-linejoin="${strokeLinejoin}" />`;
            } else {
                svg.innerHTML = `<polygon points="${pointsStr}" fill="${fill}" stroke="${stroke}" stroke-width="${strokeWidth}" stroke-linejoin="${strokeLinejoin}" />`;
            }
        }

        function initApp() {
            changeCanvasSize();
            applyMargins(); // Terapkan pembatasan margin awal
            updateZoomDisplay();
            showToast("FM Collection Print Studio siap digunakan!", "fa-circle-check");
        }

        // === HELPER UNTUK MEMBINA 8 RESIZE HANDLES ===
        function addResizeHandles(wrapper) {
            const handleConfigs = [
                { dir: 'tl', cursor: 'nwse-resize', style: 'top: -4px; left: -4px;' },
                { dir: 'tc', cursor: 'ns-resize',   style: 'top: -4px; left: calc(50% - 4px);' },
                { dir: 'tr', cursor: 'nesw-resize', style: 'top: -4px; right: -4px;' },
                { dir: 'ml', cursor: 'ew-resize',   style: 'top: calc(50% - 4px); left: -4px;' },
                { dir: 'mr', cursor: 'ew-resize',   style: 'top: calc(50% - 4px); right: -4px;' },
                { dir: 'bl', cursor: 'nesw-resize', style: 'bottom: -4px; left: -4px;' },
                { dir: 'bc', cursor: 'ns-resize',   style: 'bottom: -4px; left: calc(50% - 4px);' },
                { dir: 'br', cursor: 'se-resize',   style: 'bottom: -4px; right: -4px;' }
            ];

            handleConfigs.forEach(cfg => {
                const handle = document.createElement('div');
                handle.className = 'resize-handle';
                handle.style.cssText = `${cfg.style} cursor: ${cfg.cursor};`;
                handle.dataset.direction = cfg.dir;
                wrapper.appendChild(handle);
            });
        }

        // === FUNGSI ATUR MARGIN MODAL & LOGIC ===

        function openMarginModal() {
            document.getElementById('m-input-top').value = currentMargins.top;
            document.getElementById('m-input-bottom').value = currentMargins.bottom;
            document.getElementById('m-input-left').value = currentMargins.left;
            document.getElementById('m-input-right').value = currentMargins.right;

            document.getElementById('margin-modal').classList.remove('hidden');
        }

        function closeMarginModal() {
            document.getElementById('margin-modal').classList.add('hidden');
        }

        function setQuickMarginPreset(val) {
            document.getElementById('m-input-top').value = val;
            document.getElementById('m-input-bottom').value = val;
            document.getElementById('m-input-left').value = val;
            document.getElementById('m-input-right').value = val;
        }

        function applyMarginsFromForm() {
            const top = parseFloat(document.getElementById('m-input-top').value);
            const bottom = parseFloat(document.getElementById('m-input-bottom').value);
            const left = parseFloat(document.getElementById('m-input-left').value);
            const right = parseFloat(document.getElementById('m-input-right').value);

            const maxAllowedW = (currentCanvas.orientation === 'portrait' ? currentCanvas.widthCm : currentCanvas.heightCm) / 2;
            const maxAllowedH = (currentCanvas.orientation === 'portrait' ? currentCanvas.heightCm : currentCanvas.widthCm) / 2;

            if (isNaN(top) || isNaN(bottom) || isNaN(left) || isNaN(right) ||
                top < 0 || bottom < 0 || left < 0 || right < 0) {
                showToast("Pengaturan margin tidak valid!", "fa-triangle-exclamation", "error");
                return;
            }

            if ((left + right) >= maxAllowedW || (top + bottom) >= maxAllowedH) {
                showToast("Margin terlalu lebar untuk ukuran kertas ini!", "fa-triangle-exclamation", "error");
                return;
            }

            currentMargins.top = top;
            currentMargins.bottom = bottom;
            currentMargins.left = left;
            currentMargins.right = right;

            applyMargins();
            closeMarginModal();
            showToast("Margin berhasil diperbarui!", "fa-circle-check");
        }

        function applyMargins() {
            document.getElementById('lbl-margin-top').textContent = currentMargins.top.toFixed(1);
            document.getElementById('lbl-margin-bottom').textContent = currentMargins.bottom.toFixed(1);
            document.getElementById('lbl-margin-left').textContent = currentMargins.left.toFixed(1);
            document.getElementById('lbl-margin-right').textContent = currentMargins.right.toFixed(1);

            const topPx = Math.round(currentMargins.top * pxPerCm);
            const bottomPx = Math.round(currentMargins.bottom * pxPerCm);
            const leftPx = Math.round(currentMargins.left * pxPerCm);
            const rightPx = Math.round(currentMargins.right * pxPerCm);

            const dropzone = document.getElementById('photo-dropzone');
            dropzone.style.top = `${topPx}px`;
            dropzone.style.bottom = `${bottomPx}px`;
            dropzone.style.left = `${leftPx}px`;
            dropzone.style.right = `${rightPx}px`;

            const marginGuide = document.getElementById('margin-guide');
            marginGuide.style.top = `${topPx}px`;
            marginGuide.style.bottom = `${bottomPx}px`;
            marginGuide.style.left = `${leftPx}px`;
            marginGuide.style.right = `${rightPx}px`;
        }

        // === INTERAKSI KERTAS / KANVAS (TERKAIT CUSTOM PAPER) ===
        
        function changeCanvasSize() {
            const sizeKey = document.getElementById('canvas-size').value;
            const customInputs = document.getElementById('custom-paper-inputs');
            
            if (sizeKey === 'CUSTOM') {
                customInputs.classList.remove('hidden');
                applyCustomPaperSize();
            } else {
                customInputs.classList.add('hidden');
                const paper = paperSizes[sizeKey];
                
                currentCanvas.size = sizeKey;
                currentCanvas.widthCm = paper.w;
                currentCanvas.heightCm = paper.h;
                
                applyCanvasDimensions();
                applyMargins();
            }
        }

        function applyCustomPaperSize() {
            const wInput = parseFloat(document.getElementById('custom-paper-w').value) || 21.0;
            const hInput = parseFloat(document.getElementById('custom-paper-h').value) || 29.7;
            const unit = document.getElementById('custom-paper-unit').value;

            if (wInput <= 0 || hInput <= 0) {
                showToast("Ukuran kertas kustom tidak valid!", "fa-circle-xmark", "error");
                return;
            }

            let wCm = wInput;
            let hCm = hInput;

            if (unit === 'inch') {
                wCm = wInput * 2.54;
                hCm = hInput * 2.54;
            } else if (unit === 'mm') {
                wCm = wInput / 10;
                hCm = hInput / 10;
            }

            currentCanvas.size = 'CUSTOM';
            currentCanvas.widthCm = parseFloat(wCm.toFixed(2));
            currentCanvas.heightCm = parseFloat(hCm.toFixed(2));

            applyCanvasDimensions();
            applyMargins();
        }

        function changeOrientation(orient) {
            currentCanvas.orientation = orient;
            
            const btnP = document.getElementById('orient-portrait');
            const btnL = document.getElementById('orient-landscape');
            
            if (orient === 'portrait') {
                btnP.className = "py-1.5 px-3 rounded text-xs font-medium ps-panel border border-sky-500 text-sky-400 flex items-center justify-center gap-1";
                btnL.className = "py-1.5 px-3 rounded text-xs font-medium ps-panel border border-zinc-700 hover:border-zinc-500 flex items-center justify-center gap-1";
            } else {
                btnL.className = "py-1.5 px-3 rounded text-xs font-medium ps-panel border border-sky-500 text-sky-400 flex items-center justify-center gap-1";
                btnP.className = "py-1.5 px-3 rounded text-xs font-medium ps-panel border border-zinc-700 hover:border-zinc-500 flex items-center justify-center gap-1";
            }

            applyCanvasDimensions();
            applyMargins(); 
        }

        function applyCanvasDimensions() {
            const printArea = document.getElementById('print-area');
            let w = currentCanvas.widthCm;
            let h = currentCanvas.heightCm;

            if (currentCanvas.orientation === 'landscape') {
                const temp = w;
                w = h;
                h = temp;
            }

            const widthPx = Math.round(w * pxPerCm);
            const heightPx = Math.round(h * pxPerCm);

            printArea.style.width = `${widthPx}px`;
            printArea.style.height = `${heightPx}px`;

            document.getElementById('canvas-info-display').textContent = `${currentCanvas.size} (${w} x ${h} cm) [${currentCanvas.orientation}]`;
            
            fitCanvasToScreen(widthPx, heightPx);
        }

        function fitCanvasToScreen(widthPx, heightPx) {
            const workspace = document.querySelector('main');
            const wsWidth = workspace.clientWidth - 80;
            const wsHeight = workspace.clientHeight - 80;

            const scaleX = wsWidth / widthPx;
            const scaleY = wsHeight / heightPx;
            
            zoomScale = Math.min(scaleX, scaleY, 1.0);
            zoomScale = Math.floor(zoomScale * 20) / 20;
            if (zoomScale < 0.2) zoomScale = 0.2;

            updateZoomDisplay();
        }

        function zoomCanvas(amount) {
            zoomScale += amount;
            if (zoomScale < 0.2) zoomScale = 0.2;
            if (zoomScale > 3.0) zoomScale = 3.0;
            updateZoomDisplay();
        }

        function updateZoomDisplay() {
            const canvasWrapper = document.getElementById('canvas-wrapper');
            if (canvasWrapper) {
                canvasWrapper.style.transform = `scale(${zoomScale})`;
            }
            const zoomLabel = document.getElementById('zoom-label');
            if (zoomLabel) {
                zoomLabel.textContent = `${Math.round(zoomScale * 100)}%`;
            }
        }

        // === TAB PRESET & CUSTOM ===
        
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');

            const tabs = ['tab-pasfoto', 'tab-r', 'tab-card'];
            tabs.forEach(t => {
                const btn = document.getElementById(`btn-${t}`);
                if (t === tabId) {
                    btn.className = "py-1 rounded bg-zinc-700 text-white font-medium text-center";
                } else {
                    btn.className = "py-1 rounded text-zinc-400 hover:text-white text-center";
                }
            });
        }

        function setPhotoPreset(widthCm, heightCm, label) {
            activePreset.width = widthCm;
            activePreset.height = heightCm;
            activePreset.label = label;

            document.getElementById('selected-preset-label').textContent = `${label} (${widthCm} x ${heightCm} cm)`;
            showToast(`Preset Aktif: ${label}`, "fa-crop-simple");

            if (selectedPhoto) {
                resizePhotoElement(selectedPhoto, widthCm, heightCm);
                showToast("Saiz elemen disesuaikan ke " + label, "fa-circle-check");
            }
        }

        function updateCustomSelection() {
            const w = parseFloat(document.getElementById('custom-width').value) || 0;
            const h = parseFloat(document.getElementById('custom-height').value) || 0;
            const unit = document.getElementById('custom-unit').value;
            
            document.getElementById('selected-preset-label').textContent = `Kustom (${w}x${h} ${unit})`;
        }

        function applyCustomSize() {
            const wInput = parseFloat(document.getElementById('custom-width').value) || 0;
            const hInput = parseFloat(document.getElementById('custom-height').value) || 0;
            const unit = document.getElementById('custom-unit').value;

            if (wInput <= 0 || hInput <= 0) {
                showToast("Saiz kustom tidak sah!", "fa-circle-xmark", "error");
                return;
            }

            let wCm = wInput;
            let hCm = hInput;

            if (unit === 'inch') {
                wCm = wInput * 2.54;
                hCm = hInput * 2.54;
            } else if (unit === 'mm') {
                wCm = wInput / 10;
                hCm = hInput / 10;
            }

            activePreset.width = parseFloat(wCm.toFixed(2));
            activePreset.height = parseFloat(hCm.toFixed(2));
            activePreset.label = `Kustom (${wInput}x${hInput} ${unit})`;

            document.getElementById('selected-preset-label').textContent = `${activePreset.label} (${activePreset.width} x ${activePreset.height} cm)`;
            showToast("Dimensi Kustom Ditetapkan!", "fa-arrows-left-right-to-line");

            if (selectedPhoto) {
                resizePhotoElement(selectedPhoto, activePreset.width, activePreset.height);
            }
        }

        // === KAWALAN PAN / CROP INTERAKTIF ===

        function setDragMode(mode) {
            currentDragMode = mode;
            const btnFrame = document.getElementById('btn-mode-move-frame');
            const btnContent = document.getElementById('btn-mode-move-content');

            if (mode === 'frame') {
                btnFrame.className = "py-1 px-2 text-[10px] bg-sky-600 rounded text-white font-medium border border-sky-500 flex items-center justify-center gap-1";
                btnContent.className = "py-1 px-2 text-[10px] bg-zinc-800 hover:bg-zinc-700 rounded text-zinc-300 border border-zinc-700 flex items-center justify-center gap-1";
                if (selectedPhoto) {
                    selectedPhoto.style.cursor = 'move';
                    selectedPhoto.classList.remove('crop-mode-active');
                }
            } else {
                btnContent.className = "py-1 px-2 text-[10px] bg-sky-600 rounded text-white font-medium border border-sky-500 flex items-center justify-center gap-1";
                btnFrame.className = "py-1 px-2 text-[10px] bg-zinc-800 hover:bg-zinc-700 rounded text-zinc-300 border border-zinc-700 flex items-center justify-center gap-1";
                if (selectedPhoto) {
                    selectedPhoto.style.cursor = 'grab';
                    selectedPhoto.classList.add('crop-mode-active');
                }
            }
        }

        // Fungsi klik ganda (Double Click) interaktif untuk langsung crop / edit teks
        function handlePhotoDblClick(e) {
            const wrapper = e.currentTarget;
            if (wrapper.dataset.type === 'photo') {
                if (currentDragMode === 'frame') {
                    setDragMode('content');
                    showToast("Mode Crop/Geser Foto Aktif!", "fa-crop", "warning");
                } else {
                    setDragMode('frame');
                    showToast("Potongan foto disimpan!", "fa-circle-check");
                }
            } else if (wrapper.dataset.type === 'shape' || wrapper.dataset.type === 'text') {
                const textOverlay = wrapper.querySelector('.shape-text-overlay');
                if (textOverlay) {
                    // Sembunyikan SVG lengkung jika ada agar fokus ke pengetikan datar
                    const svgCurve = wrapper.querySelector('.text-curve-svg');
                    if (svgCurve) svgCurve.style.display = 'none';

                    textOverlay.setAttribute('contenteditable', 'true');
                    textOverlay.classList.remove('select-none');
                    textOverlay.style.opacity = '1';
                    wrapper.classList.add('crop-mode-active'); 
                    textOverlay.focus();
                    
                    const range = document.createRange();
                    range.selectNodeContents(textOverlay);
                    const sel = window.getSelection();
                    sel.removeAllRanges();
                    sel.addRange(range);
                    
                    showToast("Silakan ketik tulisan Anda!", "fa-font");
                }
            }
        }

        function updateCropProperties() {
            if (!selectedPhoto || selectedPhoto.dataset.type !== 'photo') return;

            const valX = document.getElementById('photo-crop-x').value;
            const valY = document.getElementById('photo-crop-y').value;

            document.getElementById('crop-x-val').textContent = valX + "%";
            document.getElementById('crop-y-val').textContent = valY + "%";

            selectedPhoto.dataset.cropX = valX;
            selectedPhoto.dataset.cropY = valY;

            const img = selectedPhoto.querySelector('img');
            if (img) {
                img.style.objectPosition = `${valX}% ${valY}%`;
            }
        }

        // === SELEKSI ELEMEN (FOTO, SHAPE, TEKS) ===
        function selectPhotoElement(el) {
            deselectAllPhotos();
            selectedPhoto = el;
            el.classList.add('selected');
            el.classList.add('photo-border-active');
            
            document.getElementById('no-photo-selected-msg').classList.add('hidden');
            document.getElementById('photo-selected-controls').classList.remove('hidden');

            const shapeColorControl = document.getElementById('shape-colors-control');
            const photoBorderControl = document.getElementById('photo-border-control');
            const photoCropControl = document.getElementById('photo-crop-control');

            if (el.dataset.type === 'shape' || el.dataset.type === 'text') {
                shapeColorControl.classList.remove('hidden');
                photoBorderControl.classList.add('hidden');
                photoCropControl.classList.add('hidden');

                const fillStrokeInputs = document.getElementById('shape-fill-stroke-inputs');
                const strokeWidthContainer = document.getElementById('shape-stroke-width-container');
                const btnNodeEdit = document.getElementById('btn-toggle-node-edit');
                const cornerRadiusSlider = document.getElementById('shape-corner-radius').parentElement;

                if (el.dataset.type === 'text') {
                    fillStrokeInputs.classList.add('hidden');
                    strokeWidthContainer.classList.add('hidden');
                    btnNodeEdit.classList.add('hidden');
                    cornerRadiusSlider.classList.add('hidden');
                    
                    // Muat properti spesifik teks bebas ke slider kontrol
                    document.getElementById('shape-text-curve').value = el.dataset.textCurve || '0';
                    document.getElementById('text-curve-val').textContent = el.dataset.textCurve || '0';

                    document.getElementById('shape-text-spacing').value = parseInt(el.dataset.textSpacing) || 0;
                    document.getElementById('text-spacing-val').textContent = (el.dataset.textSpacing || '0') + "px";

                    document.getElementById('shape-text-lineheight').value = (parseFloat(el.dataset.textLineHeight) * 10) || 12;
                    document.getElementById('text-lineheight-val').textContent = el.dataset.textLineHeight || '1.2';

                    document.getElementById('shape-text-shadow-blur').value = el.dataset.textShadowBlur || '0';
                    document.getElementById('shape-text-shadow-color').value = el.dataset.textShadowColor || '#000000';
                } else {
                    fillStrokeInputs.classList.remove('hidden');
                    strokeWidthContainer.classList.remove('hidden');
                    btnNodeEdit.classList.remove('hidden');
                    cornerRadiusSlider.classList.remove('hidden');
                    
                    document.getElementById('shape-fill-color').value = el.dataset.fill || '#007acc';
                    document.getElementById('shape-stroke-color').value = el.dataset.stroke || '#ffffff';
                    document.getElementById('shape-stroke-width').value = el.dataset.strokeWidth || '2';
                    document.getElementById('stroke-val').textContent = `${el.dataset.strokeWidth || '2'}px`;
                    
                    const rxVal = el.dataset.cornerRadius || '0';
                    document.getElementById('shape-corner-radius').value = rxVal;
                    document.getElementById('corner-radius-val').textContent = rxVal + "px";

                    document.getElementById('shape-text-curve').value = '0';
                    document.getElementById('text-curve-val').textContent = '0';
                }

                document.getElementById('shape-font-family').value = el.dataset.fontFamily || "'Inter', sans-serif";
                document.getElementById('shape-text-color').value = el.dataset.textColor || '#ffffff';
                document.getElementById('shape-text-size').value = el.dataset.textSize || '14';
                
                highlightTextFormatButtons();
                highlightTextAlignButtons(el.dataset.textAlign || 'center');
            } else {
                shapeColorControl.classList.add('hidden');
                photoBorderControl.classList.remove('hidden');
                photoCropControl.classList.remove('hidden');

                const cropX = Math.round(parseFloat(el.dataset.cropX) || 50);
                const cropY = Math.round(parseFloat(el.dataset.cropY) || 50);
                
                document.getElementById('photo-crop-x').value = cropX;
                document.getElementById('photo-crop-y').value = cropY;
                document.getElementById('crop-x-val').textContent = cropX + "%";
                document.getElementById('crop-y-val').textContent = cropY + "%";

                setDragMode('frame');

                const borderType = el.dataset.border || 'none';
                highlightBorderButton(borderType);
            }
        }

        // === NYAH-PILIH SEMUA ELEMEN ===
        function deselectAllPhotos() {
            if (isEditingNodes) {
                isEditingNodes = false;
                const btn = document.getElementById('btn-toggle-node-edit');
                if (btn) {
                    btn.className = "w-full py-1.5 bg-zinc-800 hover:bg-zinc-700 text-amber-400 rounded text-xs font-bold border border-zinc-700 transition flex items-center justify-center gap-1.5";
                    btn.innerHTML = `<i class="fa-solid fa-bezier-curve"></i> Aktifkan Edit Titik (Pen Tool)`;
                }
            }

            photos.forEach(el => {
                el.classList.remove('selected');
                el.classList.remove('photo-border-active');
                el.classList.remove('crop-mode-active');
                clearNodeHandles(el);

                el.querySelectorAll('.resize-handle').forEach(h => h.style.display = 'block');
                
                const txt = el.querySelector('.shape-text-overlay');
                if (txt) {
                    txt.setAttribute('contenteditable', 'false');
                    txt.classList.add('select-none');
                }
            });
            selectedPhoto = null;
            currentDragMode = 'frame';
            setDragMode('frame');

            document.getElementById('no-photo-selected-msg').classList.remove('hidden');
            document.getElementById('photo-selected-controls').classList.add('hidden');
        }

        // === PENJANAAN ELEMEN TEKS BEBAS MANDIRI ===
        function createNewTextElement(options = null) {
            const dropzone = document.getElementById('photo-dropzone');
            
            const wrapper = document.createElement('div');
            wrapper.className = 'photo-wrapper group';
            wrapper.id = 'text-' + Date.now() + '-' + Math.floor(Math.random() * 1000);
            
            wrapper.dataset.type = 'text';
            wrapper.dataset.rotation = options ? options.rotation : 0;
            wrapper.dataset.scaleX = options ? options.scaleX : 1;
            wrapper.dataset.scaleY = options ? options.scaleY : 1;
            
            wrapper.dataset.textColor = options ? options.textColor : '#38bdf8'; 
            wrapper.dataset.textSize = options ? options.textSize : '28';
            wrapper.dataset.textBold = options ? options.textBold : 'bold';
            wrapper.dataset.textItalic = options ? options.textItalic : 'normal';
            wrapper.dataset.textUnderline = options ? options.textUnderline : 'none';
            wrapper.dataset.textAlign = options ? options.textAlign : 'center';
            wrapper.dataset.fontFamily = options ? options.fontFamily : "'Montserrat', sans-serif";
            
            wrapper.dataset.textCurve = options ? options.textCurve : '0';
            wrapper.dataset.textSpacing = options ? options.textSpacing : '0';
            wrapper.dataset.textLineHeight = options ? options.textLineHeight : '1.2';
            wrapper.dataset.textShadowBlur = options ? options.textShadowBlur : '0';
            wrapper.dataset.textShadowColor = options ? options.textShadowColor : '#000000';

            const sideCmWidth = options ? parseFloat(options.widthCm) : 8.5;
            const sideCmHeight = options ? parseFloat(options.heightCm) : 3.0;
            
            const wPx = Math.round(sideCmWidth * pxPerCm);
            const hPx = Math.round(sideCmHeight * pxPerCm);
            
            wrapper.style.width = `${wPx}px`;
            wrapper.style.height = `${hPx}px`;
            
            const startX = Math.round((dropzone.clientWidth - wPx) / 2) + (photos.length * 15);
            const startY = 200 + (photos.length * 15);
            wrapper.style.left = `${startX}px`;
            wrapper.style.top = `${startY}px`;

            wrapper.dataset.widthCm = sideCmWidth;
            wrapper.dataset.heightCm = sideCmHeight;

            const textOverlay = document.createElement('div');
            textOverlay.className = 'shape-text-overlay absolute inset-0 p-2 break-words text-zinc-100 select-none z-10';
            textOverlay.setAttribute('contenteditable', 'false'); 
            textOverlay.innerHTML = options ? options.text : "FM COLLECTION"; 

            textOverlay.addEventListener('blur', () => {
                textOverlay.setAttribute('contenteditable', 'false');
                textOverlay.classList.add('select-none');
                wrapper.classList.remove('crop-mode-active');
                
                applyTextStylesAndWarp(wrapper);
                showToast("Teks berhasil diperbarui!", "fa-circle-check");
            });

            textOverlay.addEventListener('mousedown', (e) => {
                if (document.activeElement !== textOverlay) {
                    // Biarkan drag
                } else {
                    e.stopPropagation(); 
                }
            });
            textOverlay.addEventListener('touchstart', (e) => {
                if (document.activeElement !== textOverlay) {
                    // Biarkan drag
                } else {
                    e.stopPropagation();
                }
            });

            wrapper.appendChild(textOverlay);

            const actionsDiv = document.createElement('div');
            actionsDiv.className = 'absolute -top-7 left-0 right-0 flex justify-center space-x-1 opacity-0 group-hover:opacity-100 transition-opacity duration-150 pointer-events-none z-30 photo-actions';
            actionsDiv.innerHTML = `
                <div class="flex bg-[#252526] border border-zinc-700 rounded shadow-md pointer-events-auto px-1 py-0.5">
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="rotateThis('${wrapper.id}', -90)" title="Rotasi"><i class="fa-solid fa-rotate-left"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="duplicateThis('${wrapper.id}')" title="Duplikasi"><i class="fa-solid fa-copy"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-red-400 hover:text-red-300" onclick="deleteThis('${wrapper.id}')" title="Padam"><i class="fa-solid fa-trash"></i></button>
                </div>
            `;
            wrapper.appendChild(actionsDiv);

            addResizeHandles(wrapper);

            wrapper.addEventListener('mousedown', dragStart);
            wrapper.addEventListener('touchstart', dragStart, { passive: false });
            wrapper.addEventListener('dblclick', handlePhotoDblClick);

            dropzone.appendChild(wrapper);
            photos.push(wrapper);

            applyTextStylesAndWarp(wrapper);

            if (options) {
                applyPhotoTransforms(wrapper);
            }

            selectPhotoElement(wrapper);
            if (!options) {
                showToast("Teks Bebas Ditambahkan! Klik ganda untuk mengetik.", "fa-font");
            }
        }

        // === PENJANAAN BENTUK ===
        function createShapeElement(type, options = null) {
            const dropzone = document.getElementById('photo-dropzone');
            
            const wrapper = document.createElement('div');
            wrapper.className = 'photo-wrapper group';
            wrapper.id = 'shape-' + Date.now() + '-' + Math.floor(Math.random() * 1000);
            
            wrapper.dataset.type = 'shape';
            wrapper.dataset.shapeType = type;
            wrapper.dataset.fill = options ? options.fill : '#007acc'; 
            wrapper.dataset.stroke = options ? options.stroke : '#ffffff'; 
            wrapper.dataset.strokeWidth = options ? options.strokeWidth : '2';
            wrapper.dataset.strokeLinejoin = options ? options.strokeLinejoin : 'miter';
            wrapper.dataset.cornerRadius = options ? options.cornerRadius : '0';
            wrapper.dataset.rotation = options ? options.rotation : 0;
            wrapper.dataset.scaleX = options ? options.scaleX : 1;
            wrapper.dataset.scaleY = options ? options.scaleY : 1;
            
            if (options && options.points) {
                wrapper.dataset.points = options.points;
                wrapper.dataset.pointsEdited = options.pointsEdited || 'false';
            }
            
            wrapper.dataset.textColor = options ? options.textColor : '#ffffff';
            wrapper.dataset.textSize = options ? options.textSize : '14';
            wrapper.dataset.textBold = options ? options.textBold : 'normal';
            wrapper.dataset.textItalic = options ? options.textItalic : 'normal';
            wrapper.dataset.textUnderline = options ? options.textUnderline : 'none';
            wrapper.dataset.textAlign = options ? options.textAlign : 'center';
            wrapper.dataset.fontFamily = options ? options.fontFamily : "'Inter', sans-serif";

            const sideCm = options ? parseFloat(options.widthCm) : 6.0;
            const heightCm = options ? parseFloat(options.heightCm) : 6.0;
            
            const wPx = Math.round(sideCm * pxPerCm);
            const hPx = Math.round(heightCm * pxPerCm);
            
            wrapper.style.width = `${wPx}px`;
            wrapper.style.height = `${hPx}px`;
            
            const startX = Math.round((dropzone.clientWidth - wPx) / 2) + (photos.length * 15);
            const startY = 150 + (photos.length * 15);
            wrapper.style.left = `${startX}px`;
            wrapper.style.top = `${startY}px`;

            wrapper.dataset.widthCm = sideCm;
            wrapper.dataset.heightCm = heightCm;

            const svg = document.createElementNS("http://www.w3.org/2000/svg", "svg");
            svg.setAttribute("width", "100%");
            svg.setAttribute("height", "100%");
            svg.setAttribute("viewBox", "0 0 100 100");
            svg.setAttribute("preserveAspectRatio", "none");
            svg.className = "w-full h-full pointer-events-none";

            wrapper.appendChild(svg);

            getShapePoints(wrapper);
            updateSVGPath(wrapper);

            const textOverlay = document.createElement('div');
            textOverlay.className = 'shape-text-overlay absolute inset-0 p-4 break-words text-zinc-100 select-none';
            textOverlay.setAttribute('contenteditable', 'false'); 
            textOverlay.style.color = wrapper.dataset.textColor;
            textOverlay.style.fontSize = `${wrapper.dataset.textSize}px`;
            textOverlay.style.fontWeight = wrapper.dataset.textBold;
            textOverlay.style.fontStyle = wrapper.dataset.textItalic;
            textOverlay.style.textDecoration = wrapper.dataset.textUnderline;
            textOverlay.style.textAlign = wrapper.dataset.textAlign;
            textOverlay.style.fontFamily = wrapper.dataset.fontFamily;
            textOverlay.innerHTML = options ? options.text : "Teks"; 
            
            textOverlay.addEventListener('mousedown', (e) => {
                if (document.activeElement !== textOverlay) {
                    // Biarkan drag
                } else {
                    e.stopPropagation(); 
                }
            });
            textOverlay.addEventListener('touchstart', (e) => {
                if (document.activeElement !== textOverlay) {
                    // Biarkan drag
                } else {
                    e.stopPropagation();
                }
            });

            textOverlay.addEventListener('blur', () => {
                textOverlay.setAttribute('contenteditable', 'false');
                textOverlay.classList.add('select-none');
                wrapper.classList.remove('crop-mode-active');
                showToast("Teks bentuk diperbarui!", "fa-circle-check");
            });

            wrapper.appendChild(textOverlay);

            const actionsDiv = document.createElement('div');
            actionsDiv.className = 'absolute -top-7 left-0 right-0 flex justify-center space-x-1 opacity-0 group-hover:opacity-100 transition-opacity duration-150 pointer-events-none z-30 photo-actions';
            actionsDiv.innerHTML = `
                <div class="flex bg-[#252526] border border-zinc-700 rounded shadow-md pointer-events-auto px-1 py-0.5">
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="rotateThis('${wrapper.id}', -90)" title="Rotasi"><i class="fa-solid fa-rotate-left"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="duplicateThis('${wrapper.id}')" title="Duplikasi"><i class="fa-solid fa-copy"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-red-400 hover:text-red-300" onclick="deleteThis('${wrapper.id}')" title="Padam"><i class="fa-solid fa-trash"></i></button>
                </div>
            `;
            wrapper.appendChild(actionsDiv);

            addResizeHandles(wrapper);

            wrapper.addEventListener('mousedown', dragStart);
            wrapper.addEventListener('touchstart', dragStart, { passive: false });
            wrapper.addEventListener('dblclick', handlePhotoDblClick);

            dropzone.appendChild(wrapper);
            photos.push(wrapper);

            if (options) {
                applyPhotoTransforms(wrapper);
            }

            selectPhotoElement(wrapper);
            if (!options) {
                showToast(`Bentuk ${type.toUpperCase()} ditambah!`, "fa-shapes");
            }
        }

        // === DRAG & DROP LOGIC ===
        function dragStart(e) {
            if (e.type === "touchstart") {
                if (e.target.closest('.photo-actions')) return;
                e.preventDefault();
            } else {
                if (e.target.closest('.photo-actions')) return;
            }

            const wrapper = e.currentTarget;
            selectPhotoElement(wrapper);

            const isContentMode = (currentDragMode === 'content' && wrapper.dataset.type === 'photo');

            if (e.target.classList.contains('resize-handle')) {
                isResizing = true;
                resizeDirection = e.target.dataset.direction;
                
                initialWidth = wrapper.clientWidth;
                initialHeight = wrapper.clientHeight;
                initialLeft = parseFloat(wrapper.style.left) || 0;
                initialTop = parseFloat(wrapper.style.top) || 0;
                
                if (e.type === "touchstart") {
                    initialX = e.touches[0].clientX;
                    initialY = e.touches[0].clientY;
                } else {
                    initialX = e.clientX;
                    initialY = e.clientY;
                }
                
                window.addEventListener('mousemove', dragMove);
                window.addEventListener('mouseup', dragEnd);
                window.addEventListener('touchmove', dragMove, { passive: false });
                window.addEventListener('touchend', dragEnd);
                return;
            }

            if (isContentMode) {
                isDraggingContent = true;
                wrapper.style.cursor = 'grabbing';
                
                initialCropX = parseFloat(wrapper.dataset.cropX) || 50;
                initialCropY = parseFloat(wrapper.dataset.cropY) || 50;

                initialX = e.type === "touchstart" ? e.touches[0].clientX : e.clientX;
                initialY = e.type === "touchstart" ? e.touches[0].clientY : e.clientY;

                window.addEventListener('mousemove', dragMove);
                window.addEventListener('mouseup', dragEnd);
                window.addEventListener('touchmove', dragMove, { passive: false });
                window.addEventListener('touchend', dragEnd);
                return;
            }

            if (!isEditingNodes) {
                isDragging = true;
                
                const startX = e.type === "touchstart" ? e.touches[0].clientX : e.clientX;
                const startY = e.type === "touchstart" ? e.touches[0].clientY : e.clientY;
                
                const rect = wrapper.getBoundingClientRect();
                xOffset = (startX - rect.left) / zoomScale;
                yOffset = (startY - rect.top) / zoomScale;

                window.addEventListener('mousemove', dragMove);
                window.addEventListener('mouseup', dragEnd);
                window.addEventListener('touchmove', dragMove, { passive: false });
                window.addEventListener('touchend', dragEnd);
            }
        }

        function dragMove(e) {
            if (!selectedPhoto) return;
            e.preventDefault();

            const dropzone = document.getElementById('photo-dropzone');
            const dropzoneRect = dropzone.getBoundingClientRect();

            const clientX = e.type === "touchmove" ? e.touches[0].clientX : e.clientX;
            const clientY = e.type === "touchmove" ? e.touches[0].clientY : e.clientY;

            if (isResizing) {
                const deltaX = (clientX - initialX) / zoomScale;
                const deltaY = (clientY - initialY) / zoomScale;

                let newW = initialWidth;
                let newH = initialHeight;
                let newL = initialLeft;
                let newT = initialTop;

                const ratio = initialWidth / initialHeight;
                const isPhoto = (selectedPhoto.dataset.type === 'photo');

                switch (resizeDirection) {
                    case 'mr': 
                        newW = initialWidth + deltaX;
                        break;
                    case 'ml': 
                        newW = initialWidth - deltaX;
                        newL = initialLeft + deltaX;
                        break;
                    case 'bc': 
                        newH = initialHeight + deltaY;
                        break;
                    case 'tc': 
                        newH = initialHeight - deltaY;
                        newT = initialTop + deltaY;
                        break;
                    case 'br': 
                        newW = initialWidth + deltaX;
                        newH = isPhoto ? (newW / ratio) : (initialHeight + deltaY);
                        break;
                    case 'bl': 
                        newW = initialWidth - deltaX;
                        newL = initialLeft + deltaX;
                        newH = isPhoto ? (newW / ratio) : (initialHeight + deltaY);
                        break;
                    case 'tr': 
                        newW = initialWidth + deltaX;
                        newH = isPhoto ? (newW / ratio) : (initialHeight - deltaY);
                        newT = isPhoto ? (initialTop + (initialHeight - newH)) : (initialTop + deltaY);
                        break;
                    case 'tl': 
                        newW = initialWidth - deltaX;
                        newL = initialLeft + deltaX;
                        newH = isPhoto ? (newW / ratio) : (initialHeight - deltaY);
                        newT = isPhoto ? (initialTop + (initialHeight - newH)) : (initialTop + deltaY);
                        break;
                }

                const minSize = 20;
                if (newW >= minSize && newH >= minSize) {
                    selectedPhoto.style.width = `${Math.round(newW)}px`;
                    selectedPhoto.style.height = `${Math.round(newH)}px`;
                    selectedPhoto.style.left = `${Math.round(newL)}px`;
                    selectedPhoto.style.top = `${Math.round(newT)}px`;
                    
                    selectedPhoto.dataset.widthCm = (newW / pxPerCm).toFixed(2);
                    selectedPhoto.dataset.heightCm = (newH / pxPerCm).toFixed(2);
                    
                    if (selectedPhoto.dataset.type === 'text') {
                        applyTextStylesAndWarp(selectedPhoto);
                    }

                    if (isEditingNodes) {
                        renderNodeHandles(selectedPhoto);
                    }
                }
            } 
            else if (isDraggingContent) {
                const deltaX = (clientX - initialX) / zoomScale;
                const deltaY = (clientY - initialY) / zoomScale;

                const percentShiftX = (deltaX / selectedPhoto.clientWidth) * 100;
                const percentShiftY = (deltaY / selectedPhoto.clientHeight) * 100;

                let newCropX = initialCropX - percentShiftX;
                let newCropY = initialCropY - percentShiftY;

                newCropX = Math.max(0, Math.min(100, newCropX));
                newCropY = Math.max(0, Math.min(100, newCropY));

                selectedPhoto.dataset.cropX = newCropX;
                selectedPhoto.dataset.cropY = newCropY;

                const img = selectedPhoto.querySelector('img');
                if (img) {
                    img.style.objectPosition = `${newCropX}% ${newCropY}%`;
                }

                document.getElementById('photo-crop-x').value = Math.round(newCropX);
                document.getElementById('photo-crop-y').value = Math.round(newCropY);
                document.getElementById('crop-x-val').textContent = `${Math.round(newCropX)}%`;
                document.getElementById('crop-y-val').textContent = `${Math.round(newCropY)}%`;
            } 
            else if (isDragging) {
                const targetX = (clientX - dropzoneRect.left) / zoomScale - xOffset;
                const targetY = (clientY - dropzoneRect.top) / zoomScale - yOffset;

                const minX = -selectedPhoto.clientWidth + 20;
                const maxX = dropzone.clientWidth - 20;
                const minY = -selectedPhoto.clientHeight + 20;
                const maxY = dropzone.clientHeight - 20;

                const finalX = Math.max(minX, Math.min(targetX, maxX));
                const finalY = Math.max(minY, Math.min(targetY, maxY));

                selectedPhoto.style.left = `${Math.round(finalX)}px`;
                selectedPhoto.style.top = `${Math.round(finalY)}px`;
            }
        }

        function dragEnd() {
            isDragging = false;
            isResizing = false;
            
            if (isDraggingContent && selectedPhoto) {
                selectedPhoto.style.cursor = 'grab';
            }
            isDraggingContent = false;
            
            window.removeEventListener('mousemove', dragMove);
            window.removeEventListener('mouseup', dragEnd);
            window.removeEventListener('touchmove', dragMove);
            window.removeEventListener('touchend', dragEnd);
        }

        // === TAMPILKAN GAYA & KELENGKUNGAN TEKS ===
        function applyTextStylesAndWarp(el) {
            if (!el || (el.dataset.type !== 'text' && el.dataset.type !== 'shape')) return;

            const textOverlay = el.querySelector('.shape-text-overlay');
            if (!textOverlay) return;

            const color = el.dataset.textColor || '#ffffff';
            const size = parseInt(el.dataset.textSize) || 14;
            const font = el.dataset.fontFamily || "'Inter', sans-serif";
            const bold = el.dataset.textBold || 'normal';
            const italic = el.dataset.textItalic || 'normal';
            const underline = el.dataset.textUnderline || 'none';
            const align = el.dataset.textAlign || 'center';
            
            const curve = parseInt(el.dataset.textCurve) || 0;
            const letterSpacing = (el.dataset.textSpacing || '0') + "px";
            const lineHeight = el.dataset.textLineHeight || '1.2';
            
            const shadowBlur = parseInt(el.dataset.textShadowBlur) || 0;
            const shadowColor = el.dataset.textShadowColor || '#000000';

            const oldSvg = el.querySelector('.text-curve-svg');
            if (oldSvg) oldSvg.remove();

            textOverlay.style.color = color;
            textOverlay.style.fontSize = `${size}px`;
            textOverlay.style.fontFamily = font;
            textOverlay.style.fontWeight = bold;
            textOverlay.style.fontStyle = italic;
            textOverlay.style.textDecoration = underline;
            textOverlay.style.textAlign = align;
            textOverlay.style.letterSpacing = letterSpacing;
            textOverlay.style.lineHeight = lineHeight;
            
            if (shadowBlur > 0) {
                textOverlay.style.textShadow = `1px 1px ${shadowBlur}px ${shadowColor}, -1px -1px ${shadowBlur}px ${shadowColor}`;
            } else {
                textOverlay.style.textShadow = 'none';
            }

            if (el.dataset.type === 'text' && curve !== 0) {
                textOverlay.style.opacity = '0';
                textOverlay.setAttribute('contenteditable', 'false');

                const rawText = textOverlay.textContent || "FM COLLECTION";

                const svgNS = "http://www.w3.org/2000/svg";
                const svg = document.createElementNS(svgNS, "svg");
                svg.setAttribute("class", "text-curve-svg absolute inset-0 w-full h-full pointer-events-none");
                svg.setAttribute("viewBox", "0 0 200 100");
                svg.setAttribute("preserveAspectRatio", "none");

                const startY = 50 + (curve / 4.5);
                const controlY = 50 - (curve * 0.95);
                const endY = 50 + (curve / 4.5);

                const path = document.createElementNS(svgNS, "path");
                path.setAttribute("id", "curve-path-" + el.id);
                path.setAttribute("d", `M 10,${startY} Q 100,${controlY} 190,${endY}`);
                path.setAttribute("fill", "none");
                svg.appendChild(path);

                const textNode = document.createElementNS(svgNS, "text");
                textNode.setAttribute("fill", color);
                textNode.setAttribute("font-size", `${size * 0.65}px`); 
                textNode.setAttribute("font-family", font);
                textNode.setAttribute("font-weight", bold);
                textNode.setAttribute("font-style", italic);
                textNode.setAttribute("text-anchor", "middle");
                textNode.setAttribute("letter-spacing", letterSpacing);
                
                if (shadowBlur > 0) {
                    textNode.setAttribute("style", `filter: drop-shadow(1px 1px ${shadowBlur / 2}px ${shadowColor});`);
                }

                const textPath = document.createElementNS(svgNS, "textPath");
                textPath.setAttributeNS("http://www.w3.org/1999/xlink", "xlink:href", "#curve-path-" + el.id);
                textPath.setAttribute("startOffset", "50%");
                textPath.textContent = rawText;

                textNode.appendChild(textPath);
                svg.appendChild(textNode);
                el.appendChild(svg);
            } else {
                textOverlay.style.opacity = '1';
            }
        }

        // Pembaruan Gaya Teks Bentuk & Text Layer Real-time
        function updateShapeTextProperties() {
            if (!selectedPhoto || (selectedPhoto.dataset.type !== 'shape' && selectedPhoto.dataset.type !== 'text')) return;

            const textColor = document.getElementById('shape-text-color').value;
            const textSize = document.getElementById('shape-text-size').value;
            const fontFamily = document.getElementById('shape-font-family').value;
            
            const textCurve = document.getElementById('shape-text-curve').value;
            const textSpacing = document.getElementById('shape-text-spacing').value;
            const textLineHeight = (document.getElementById('shape-text-lineheight').value / 10).toFixed(1);
            
            const shadowColor = document.getElementById('shape-text-shadow-color').value;
            const shadowBlur = document.getElementById('shape-text-shadow-blur').value;

            selectedPhoto.dataset.textColor = textColor;
            selectedPhoto.dataset.textSize = textSize;
            selectedPhoto.dataset.fontFamily = fontFamily;
            
            selectedPhoto.dataset.textCurve = textCurve;
            selectedPhoto.dataset.textSpacing = textSpacing;
            selectedPhoto.dataset.textLineHeight = textLineHeight;
            
            selectedPhoto.dataset.textShadowColor = shadowColor;
            selectedPhoto.dataset.textShadowBlur = shadowBlur;

            document.getElementById('text-curve-val').textContent = textCurve;
            document.getElementById('text-spacing-val').textContent = textSpacing + "px";
            document.getElementById('text-lineheight-val').textContent = textLineHeight;

            applyTextStylesAndWarp(selectedPhoto);
        }

        function toggleTextFormat(format) {
            if (!selectedPhoto || (selectedPhoto.dataset.type !== 'shape' && selectedPhoto.dataset.type !== 'text')) return;
            const textOverlay = selectedPhoto.querySelector('.shape-text-overlay');
            if (!textOverlay) return;

            if (format === 'bold') {
                const isBold = selectedPhoto.dataset.textBold === 'bold';
                selectedPhoto.dataset.textBold = isBold ? 'normal' : 'bold';
            } else if (format === 'italic') {
                const isItalic = selectedPhoto.dataset.textItalic === 'italic';
                selectedPhoto.dataset.textItalic = isItalic ? 'normal' : 'italic';
            } else if (format === 'underline') {
                const isUnderline = selectedPhoto.dataset.textUnderline === 'underline';
                selectedPhoto.dataset.textUnderline = isUnderline ? 'none' : 'underline';
            }
            
            highlightTextFormatButtons();
            applyTextStylesAndWarp(selectedPhoto);
        }

        // === CETAKAN SISTEM PRINTER / PDF ===
        function printCanvas() {
            deselectAllPhotos();
            
            const prevZoom = zoomScale;
            // Skala diset ke 1.0 (100% piksel asli layar) supaya cetakan tidak mengkerut / terpotong
            zoomScale = 1.0;
            updateZoomDisplay();

            // Beri selang waktu 300ms agar rendering CSS @media print berjalan sempurna di memori browser
            setTimeout(() => {
                window.print();
                // Kembalikan tingkat Zoom Workspace pengguna ke pengaturan semula
                zoomScale = prevZoom;
                updateZoomDisplay();
            }, 300);
        }

        function setTextAlignment(align) {
            if (!selectedPhoto || (selectedPhoto.dataset.type !== 'shape' && selectedPhoto.dataset.type !== 'text')) return;

            selectedPhoto.dataset.textAlign = align;
            highlightTextAlignButtons(align);
            applyTextStylesAndWarp(selectedPhoto);
        }

        // === INTEGRASI GAMBAR / FOTO ===
        function triggerUpload() {
            document.getElementById('image-upload').click();
        }

        function handleImageUpload(event) {
            const files = event.target.files;
            if (!files.length) return;

            Array.from(files).forEach((file, index) => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const offset = index * 20;
                    createPhotoElement(e.target.result, activePreset.width, activePreset.height, offset);
                }
                reader.readAsDataURL(file);
            });

            event.target.value = '';
        }

        function createPhotoElement(src, widthCm, heightCm, offset = 0) {
            const dropzone = document.getElementById('photo-dropzone');
            
            const wrapper = document.createElement('div');
            wrapper.className = 'photo-wrapper group';
            wrapper.id = 'photo-' + Date.now() + '-' + Math.floor(Math.random() * 1000);
            wrapper.dataset.type = 'photo';
            
            const wPx = Math.round(widthCm * pxPerCm);
            const hPx = Math.round(heightCm * pxPerCm);
            
            wrapper.style.width = `${wPx}px`;
            wrapper.style.height = `${hPx}px`;
            
            const startX = Math.max(20 + offset, Math.round((dropzone.clientWidth - wPx) / 2) + offset);
            const startY = Math.max(40 + offset, 100 + offset);
            
            wrapper.style.left = `${startX}px`;
            wrapper.style.top = `${startY}px`;

            wrapper.dataset.widthCm = widthCm;
            wrapper.dataset.heightCm = heightCm;
            wrapper.dataset.rotation = 0;
            wrapper.dataset.scaleX = 1;
            wrapper.dataset.scaleY = 1;
            wrapper.dataset.border = 'none';
            
            wrapper.dataset.cropX = 50;
            wrapper.dataset.cropY = 50;

            const img = document.createElement('img');
            img.src = src;
            img.className = 'w-full h-full object-cover transition-transform duration-100 pointer-events-none';
            img.style.transform = 'rotate(0deg) scale(1, 1)';
            img.style.objectPosition = '50% 50%';
            
            wrapper.appendChild(img);

            const actionsDiv = document.createElement('div');
            actionsDiv.className = 'absolute -top-7 left-0 right-0 flex justify-center space-x-1 opacity-0 group-hover:opacity-100 transition-opacity duration-150 pointer-events-none z-30 photo-actions';
            actionsDiv.innerHTML = `
                <div class="flex bg-[#252526] border border-zinc-700 rounded shadow-md pointer-events-auto px-1 py-0.5">
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="rotateThis('${wrapper.id}', -90)" title="Rotasi"><i class="fa-solid fa-rotate-left"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-zinc-300 hover:text-white" onclick="duplicateThis('${wrapper.id}')" title="Duplikasi"><i class="fa-solid fa-copy"></i></button>
                    <span class="text-zinc-600 px-0.5">|</span>
                    <button class="px-1 text-[10px] text-red-400 hover:text-red-300" onclick="deleteThis('${wrapper.id}')" title="Padam"><i class="fa-solid fa-trash"></i></button>
                </div>
            `;
            wrapper.appendChild(actionsDiv);

            addResizeHandles(wrapper);

            wrapper.addEventListener('mousedown', dragStart);
            wrapper.addEventListener('touchstart', dragStart, { passive: false });
            wrapper.addEventListener('dblclick', handlePhotoDblClick);

            dropzone.appendChild(wrapper);
            photos.push(wrapper);

            selectPhotoElement(wrapper);
            showToast("Foto ditambahkan (" + widthCm + " x " + heightCm + " cm)", "fa-circle-plus");
        }

        // === DESIGN SUITES UTILITY ===
        function duplicateSelected() {
            if (!selectedPhoto) return;
            
            if (selectedPhoto.dataset.type === 'shape') {
                const textVal = selectedPhoto.querySelector('.shape-text-overlay').innerHTML;
                createShapeElement(selectedPhoto.dataset.shapeType, {
                    text: textVal,
                    fill: selectedPhoto.dataset.fill,
                    stroke: selectedPhoto.dataset.stroke,
                    strokeWidth: selectedPhoto.dataset.strokeWidth,
                    strokeLinejoin: selectedPhoto.dataset.strokeLinejoin,
                    cornerRadius: selectedPhoto.dataset.cornerRadius,
                    rotation: selectedPhoto.dataset.rotation,
                    scaleX: selectedPhoto.dataset.scaleX,
                    scaleY: selectedPhoto.dataset.scaleY,
                    textColor: selectedPhoto.dataset.textColor,
                    textSize: selectedPhoto.dataset.textSize,
                    textBold: selectedPhoto.dataset.textBold,
                    textItalic: selectedPhoto.dataset.textItalic,
                    textUnderline: selectedPhoto.dataset.textUnderline,
                    textAlign: selectedPhoto.dataset.textAlign,
                    fontFamily: selectedPhoto.dataset.fontFamily,
                    widthCm: selectedPhoto.dataset.widthCm,
                    heightCm: selectedPhoto.dataset.heightCm,
                    points: selectedPhoto.dataset.points, 
                    pointsEdited: selectedPhoto.dataset.pointsEdited
                });
            } else if (selectedPhoto.dataset.type === 'text') {
                const textVal = selectedPhoto.querySelector('.shape-text-overlay').innerHTML;
                createNewTextElement({
                    text: textVal,
                    rotation: selectedPhoto.dataset.rotation,
                    scaleX: selectedPhoto.dataset.scaleX,
                    scaleY: selectedPhoto.dataset.scaleY,
                    textColor: selectedPhoto.dataset.textColor,
                    textSize: selectedPhoto.dataset.textSize,
                    textBold: selectedPhoto.dataset.textBold,
                    textItalic: selectedPhoto.dataset.textItalic,
                    textUnderline: selectedPhoto.dataset.textUnderline,
                    textAlign: selectedPhoto.dataset.textAlign,
                    fontFamily: selectedPhoto.dataset.fontFamily,
                    widthCm: selectedPhoto.dataset.widthCm,
                    heightCm: selectedPhoto.dataset.heightCm,
                    textCurve: selectedPhoto.dataset.textCurve,
                    textSpacing: selectedPhoto.dataset.textSpacing,
                    textLineHeight: selectedPhoto.dataset.textLineHeight,
                    textShadowBlur: selectedPhoto.dataset.textShadowBlur,
                    textShadowColor: selectedPhoto.dataset.textShadowColor
                });
            } else {
                const src = selectedPhoto.querySelector('img').src;
                const wCm = parseFloat(selectedPhoto.dataset.widthCm);
                const hCm = parseFloat(selectedPhoto.dataset.heightCm);
                createPhotoElement(src, wCm, hCm, 25);
            }
            showToast("Objek diduplikasi", "fa-copy");
        }

        function duplicateThis(id) {
            const el = document.getElementById(id);
            if (el) {
                if (el.dataset.type === 'shape') {
                    const textVal = el.querySelector('.shape-text-overlay').innerHTML;
                    createShapeElement(el.dataset.shapeType, {
                        text: textVal,
                        fill: el.dataset.fill,
                        stroke: el.dataset.stroke,
                        strokeWidth: el.dataset.strokeWidth,
                        strokeLinejoin: el.dataset.strokeLinejoin,
                        cornerRadius: el.dataset.cornerRadius,
                        rotation: el.dataset.rotation,
                        scaleX: el.dataset.scaleX,
                        scaleY: el.dataset.scaleY,
                        textColor: el.dataset.textColor,
                        textSize: el.dataset.textSize,
                        textBold: el.dataset.textBold,
                        textItalic: el.dataset.textItalic,
                        textUnderline: el.dataset.textUnderline,
                        textAlign: el.dataset.textAlign,
                        fontFamily: el.dataset.fontFamily,
                        widthCm: el.dataset.widthCm,
                        heightCm: el.dataset.heightCm,
                        points: el.dataset.points, 
                        pointsEdited: el.dataset.pointsEdited
                    });
                } else if (el.dataset.type === 'text') {
                    const textVal = el.querySelector('.shape-text-overlay').innerHTML;
                    createNewTextElement({
                        text: textVal,
                        rotation: el.dataset.rotation,
                        scaleX: el.dataset.scaleX,
                        scaleY: el.dataset.scaleY,
                        textColor: el.dataset.textColor,
                        textSize: el.dataset.textSize,
                        textBold: el.dataset.textBold,
                        textItalic: el.dataset.textItalic,
                        textUnderline: el.dataset.textUnderline,
                        textAlign: el.dataset.textAlign,
                        fontFamily: el.dataset.fontFamily,
                        widthCm: el.dataset.widthCm,
                        heightCm: el.dataset.heightCm,
                        textCurve: el.dataset.textCurve,
                        textSpacing: el.dataset.textSpacing,
                        textLineHeight: el.dataset.textLineHeight,
                        textShadowBlur: el.dataset.textShadowBlur,
                        textShadowColor: el.dataset.textShadowColor
                    });
                } else {
                    const src = el.querySelector('img').src;
                    const wCm = parseFloat(el.dataset.widthCm);
                    const hCm = parseFloat(el.dataset.heightCm);
                    createPhotoElement(src, wCm, hCm, 25);
                }
                showToast("Objek diduplikasi", "fa-copy");
            }
        }

        function autoLayoutPhotos() {
            if (photos.length === 0) {
                showToast("Tiada elemen untuk diatur!", "fa-triangle-exclamation", "error");
                return;
            }

            const dropzone = document.getElementById('photo-dropzone');
            const paperWidth = dropzone.clientWidth;
            
            let currentX = 15;
            let currentY = 15;
            let maxHeightInRow = 0;

            photos.forEach(photo => {
                const w = photo.clientWidth;
                const h = photo.clientHeight;

                if (currentX + w > paperWidth - 15) {
                    currentX = 15;
                    currentY += maxHeightInRow + 15;
                    maxHeightInRow = 0;
                }

                photo.style.left = `${currentX}px`;
                photo.style.top = `${currentY}px`;

                currentX += w + 15;
                if (h > maxHeightInRow) {
                    maxHeightInRow = h;
                }
            });

            showToast("Susunan elemen dirapikan!", "fa-table-cells");
        }

        function clearCanvas() {
            if (confirm("Adakah anda pasti ingin memadam semua kandungan kanvas?")) {
                const dropzone = document.getElementById('photo-dropzone');
                dropzone.innerHTML = '';
                photos = [];
                deselectAllPhotos();
                showToast("Kanvas dikosongkan", "fa-trash", "error");
            }
        }

        function resetCanvas() {
            if (confirm("Bina fail baru? Semua hasil kerja semasa akan dibuang.")) {
                const dropzone = document.getElementById('photo-dropzone');
                dropzone.innerHTML = '';
                photos = [];
                deselectAllPhotos();
                
                document.getElementById('canvas-size').value = 'A4';
                currentCanvas.size = 'A4';
                currentCanvas.orientation = 'portrait';
                currentCanvas.widthCm = 21.0;
                currentCanvas.heightCm = 29.7;
                applyCanvasDimensions();

                currentMargins = { top: 1.0, bottom: 1.0, left: 1.0, right: 1.0 };
                applyMargins();

                showToast("Kanvas Kerja Baru Dibina", "fa-file");
            }
        }

        // === FUNGSI SOROTAN VISUAL FORMAT TEKS ===
        function highlightTextFormatButtons() {
            if (!selectedPhoto) return;
            
            const isBold = selectedPhoto.dataset.textBold === 'bold';
            const isItalic = selectedPhoto.dataset.textItalic === 'italic';
            const isUnderline = selectedPhoto.dataset.textUnderline === 'underline';

            const btnBold = document.getElementById('btn-text-bold');
            const btnItalic = document.getElementById('btn-text-italic');
            const btnUnderline = document.getElementById('btn-text-underline');

            if (btnBold) {
                btnBold.className = `flex-1 py-1 rounded border text-xs font-bold ${isBold ? 'bg-sky-600 text-white border-sky-500' : 'bg-zinc-800 text-zinc-300 border-zinc-700'}`;
            }
            if (btnItalic) {
                btnItalic.className = `flex-1 py-1 rounded border text-xs italic ${isItalic ? 'bg-sky-600 text-white border-sky-500' : 'bg-zinc-800 text-zinc-300 border-zinc-700'}`;
            }
            if (btnUnderline) {
                btnUnderline.className = `flex-1 py-1 rounded border text-xs underline ${isUnderline ? 'bg-sky-600 text-white border-sky-500' : 'bg-zinc-800 text-zinc-300 border-zinc-700'}`;
            }
        }

        function highlightTextAlignButtons(align) {
            ['left', 'center', 'right'].forEach(a => {
                const btn = document.getElementById(`btn-align-${a}`);
                if (btn) {
                    if (a === align) {
                        btn.className = "px-2 py-1 bg-sky-600 border border-sky-500 rounded text-white text-xs";
                    } else {
                        btn.className = "px-2 py-1 bg-zinc-800 border border-zinc-700 rounded text-zinc-300 text-xs";
                    }
                }
            });
        }

        // === NOTIFIKASI TOAST ===
        function showToast(message, iconClass = "fa-circle-check", type = "success") {
            const toast = document.getElementById('toast-message');
            const text = document.getElementById('toast-text');
            const icon = document.getElementById('toast-icon');

            text.textContent = message;
            icon.className = `fa-solid ${iconClass}`;
            
            if (type === 'error') {
                toast.className = "fixed bottom-4 left-1/2 transform -translate-x-1/2 bg-red-950 border border-red-800 px-4 py-2 rounded-lg text-xs shadow-xl z-50 flex items-center gap-2 text-red-200";
            } else if (type === 'warning') {
                toast.className = "fixed bottom-4 left-1/2 transform -translate-x-1/2 bg-amber-950 border border-amber-800 px-4 py-2 rounded-lg text-xs shadow-xl z-50 flex items-center gap-2 text-amber-200";
            } else {
                toast.className = "fixed bottom-4 left-1/2 transform -translate-x-1/2 bg-zinc-800 border border-zinc-700 px-4 py-2 rounded-lg text-xs shadow-xl z-50 flex items-center gap-2 text-emerald-400";
            }

            toast.classList.remove('hidden');

            clearTimeout(toast.timeoutId);
            toast.timeoutId = setTimeout(() => {
                toast.classList.add('hidden');
            }, 3000);
        }

        // === FUNGSI LOGIKA VECTOR NODE EDITOR & RENDERING ===
        function toggleNodeEditMode() {
            if (!selectedPhoto || selectedPhoto.dataset.type !== 'shape') return;
            isEditingNodes = !isEditingNodes;

            const btn = document.getElementById('btn-toggle-node-edit');
            if (isEditingNodes) {
                btn.className = "w-full py-1.5 bg-amber-600 hover:bg-amber-500 text-white rounded text-xs font-bold transition flex items-center justify-center gap-1.5 shadow-lg";
                btn.innerHTML = `<i class="fa-solid fa-vector-square"></i> Matikan Edit Titik`;
                renderNodeHandles(selectedPhoto);
                selectedPhoto.querySelectorAll('.resize-handle').forEach(h => h.style.display = 'none');
                showToast("Mode Edit Titik Aktif! Tarik bulatan oranye untuk mereshape bentuk.", "fa-bezier-curve", "warning");
            } else {
                btn.className = "w-full py-1.5 bg-zinc-800 hover:bg-zinc-700 text-amber-400 rounded text-xs font-bold border border-zinc-700 transition flex items-center justify-center gap-1.5";
                btn.innerHTML = `<i class="fa-solid fa-bezier-curve"></i> Aktifkan Edit Titik (Pen Tool)`;
                clearNodeHandles(selectedPhoto);
                selectedPhoto.querySelectorAll('.resize-handle').forEach(h => h.style.display = 'block');
                showToast("Mode Edit Titik dinonaktifkan.", "fa-circle-check");
            }
        }

        function renderNodeHandles(el) {
            clearNodeHandles(el);
            const points = getShapePoints(el);
            if (!points || points.length === 0) return;

            points.forEach((pt, idx) => {
                const handle = document.createElement('div');
                handle.className = 'node-handle';
                handle.style.left = `${pt[0]}%`;
                handle.style.top = `${pt[1]}%`;
                handle.dataset.index = idx;

                handle.addEventListener('mousedown', startDragNode);
                handle.addEventListener('touchstart', startDragNode, { passive: false });
                el.appendChild(handle);
            });
        }

        function clearNodeHandles(el) {
            if (!el) return;
            el.querySelectorAll('.node-handle').forEach(h => h.remove());
        }

        function startDragNode(e) {
            e.stopPropagation();
            e.preventDefault();

            draggedNodeHandle = e.target;

            window.addEventListener('mousemove', dragNodeMove);
            window.addEventListener('mouseup', dragNodeEnd);
            window.addEventListener('touchmove', dragNodeMove, { passive: false });
            window.addEventListener('touchend', dragNodeEnd);
        }

        function dragNodeMove(e) {
            if (!draggedNodeHandle || !selectedPhoto) return;
            e.preventDefault();

            const rect = selectedPhoto.getBoundingClientRect();
            const clientX = e.type === "touchmove" ? e.touches[0].clientX : e.clientX;
            const clientY = e.type === "touchmove" ? e.touches[0].clientY : e.clientY;

            let pctX = ((clientX - rect.left) / rect.width) * 100;
            let pctY = ((clientY - rect.top) / rect.height) * 100;

            pctX = Math.max(0, Math.min(100, pctX));
            pctY = Math.max(0, Math.min(100, pctY));

            const idx = parseInt(draggedNodeHandle.dataset.index);
            const points = getShapePoints(selectedPhoto);

            points[idx] = [parseFloat(pctX.toFixed(1)), parseFloat(pctY.toFixed(1))];
            selectedPhoto.dataset.points = JSON.stringify(points);
            selectedPhoto.dataset.pointsEdited = 'true';

            draggedNodeHandle.style.left = `${pctX}%`;
            draggedNodeHandle.style.top = `${pctY}%`;

            updateSVGPath(selectedPhoto);
        }

        // Mematikan seret titik node
        function dragNodeEnd() {
            draggedNodeHandle = null;
            window.removeEventListener('mousemove', dragNodeMove);
            window.removeEventListener('mouseup', dragNodeEnd);
            window.removeEventListener('touchmove', dragNodeMove);
            window.removeEventListener('touchend', dragEnd);
        }

        function updateShapeCornerRadius() {
            if (!selectedPhoto || selectedPhoto.dataset.type !== 'shape') return;
            const rx = document.getElementById('shape-corner-radius').value;
            document.getElementById('corner-radius-val').textContent = rx + "px";

            selectedPhoto.dataset.cornerRadius = rx;
            
            if (selectedPhoto.dataset.shapeType !== 'rect') {
                selectedPhoto.dataset.strokeLinejoin = rx > 0 ? 'round' : 'miter';
            }

            updateSVGPath(selectedPhoto);
        }

        // === KONTROL PENGHAPUSAN DAN TRANSFORMASI OBJEK ===
        function deleteSelected() {
            if (!selectedPhoto) return;
            const index = photos.indexOf(selectedPhoto);
            if (index > -1) {
                photos.splice(index, 1);
            }
            selectedPhoto.remove();
            deselectAllPhotos();
            showToast("Objek berhasil dihapus", "fa-trash-can", "error");
        }

        function deleteThis(id) {
            const el = document.getElementById(id);
            if (el) {
                const index = photos.indexOf(el);
                if (index > -1) photos.splice(index, 1);
                if (selectedPhoto === el) deselectAllPhotos();
                el.remove();
                showToast("Objek berhasil dihapus", "fa-trash-can", "error");
            }
        }

        function rotateThis(id, deg) {
            const el = document.getElementById(id);
            if (el) {
                let currentRot = parseInt(el.dataset.rotation) || 0;
                let newRot = (currentRot + deg) % 360;
                el.dataset.rotation = newRot;
                applyPhotoTransforms(el);
            }
        }

        function flipSelected(axis) {
            if (!selectedPhoto) return;
            
            let scaleX = parseInt(selectedPhoto.dataset.scaleX) || 1;
            let scaleY = parseInt(selectedPhoto.dataset.scaleY) || 1;

            if (axis === 'X') {
                scaleX = scaleX * -1;
                selectedPhoto.dataset.scaleX = scaleX;
            } else {
                scaleY = scaleY * -1;
                selectedPhoto.dataset.scaleY = scaleY;
            }

            applyPhotoTransforms(selectedPhoto);
        }

        function applyPhotoTransforms(el) {
            const innerElement = el.querySelector('img') || el.querySelector('svg');
            const rot = el.dataset.rotation || 0;
            const sx = el.dataset.scaleX || 1;
            const sy = el.dataset.scaleY || 1;

            if (innerElement) {
                innerElement.style.transform = `rotate(${rot}deg) scale(${sx}, ${sy})`;
            }
        }

        function alignSelected(dir) {
            if (!selectedPhoto) return;

            const dropzone = document.getElementById('photo-dropzone');
            const parentWidth = dropzone.clientWidth;
            const parentHeight = dropzone.clientHeight;
            const w = selectedPhoto.clientWidth;
            const h = selectedPhoto.clientHeight;

            if (dir === 'left') {
                selectedPhoto.style.left = '0px';
            } else if (dir === 'center') {
                selectedPhoto.style.left = `${Math.round((parentWidth - w) / 2)}px`;
            } else if (dir === 'right') {
                selectedPhoto.style.left = `${parentWidth - w}px`;
            } else if (dir === 'top') {
                selectedPhoto.style.top = '0px';
            }
        }

        function setSelectedBorder(type) {
            if (!selectedPhoto || selectedPhoto.dataset.type === 'shape' || selectedPhoto.dataset.type === 'text') return;

            selectedPhoto.dataset.border = type;
            highlightBorderButton(type);

            selectedPhoto.style.border = '';
            
            if (type === 'white') {
                selectedPhoto.style.border = '4px solid white';
                selectedPhoto.style.boxShadow = '0 0 1px rgba(0,0,0,0.5)';
            } else if (type === 'black') {
                selectedPhoto.style.border = '1.5px solid black';
                selectedPhoto.style.boxShadow = 'none';
            } else {
                selectedPhoto.style.border = '';
                selectedPhoto.style.boxShadow = 'none';
            }
        }

        // === FUNGSI PEMBARUAN PROPERTI BENTUK ===
        function updateShapeProperties() {
            if (!selectedPhoto || selectedPhoto.dataset.type !== 'shape') return;

            const fill = document.getElementById('shape-fill-color').value;
            const stroke = document.getElementById('shape-stroke-color').value;
            const strokeWidth = document.getElementById('shape-stroke-width').value;

            document.getElementById('stroke-val').textContent = strokeWidth + "px";

            selectedPhoto.dataset.fill = fill;
            selectedPhoto.dataset.stroke = stroke;
            selectedPhoto.dataset.strokeWidth = strokeWidth;

            updateSVGPath(selectedPhoto);
        }

        // === FUNGSI PEMBARUAN STATUS VISUAL TOMBOL BORDER ===
        function highlightBorderButton(type) {
            ['none', 'white', 'black'].forEach(t => {
                const btn = document.getElementById(`border-btn-${t}`);
                if (btn) {
                    if (t === type) {
                        btn.className = "p-1 text-[10px] bg-sky-600 rounded text-white font-medium border border-sky-500";
                    } else {
                        btn.className = "p-1 text-[10px] bg-zinc-800 rounded border border-zinc-700 hover:bg-zinc-700 text-zinc-300";
                    }
                }
            });
        }

        // === FUNGSI ATUR UKURAN ELEMEN SECARA DINAMIS (PERBAIKAN UTAMA) ===
        function resizePhotoElement(el, newWidthCm, newHeightCm) {
            const wPx = Math.round(newWidthCm * pxPerCm);
            const hPx = Math.round(newHeightCm * pxPerCm);
            
            el.style.width = `${wPx}px`;
            el.style.height = `${hPx}px`;
            
            el.dataset.widthCm = newWidthCm;
            el.dataset.heightCm = newHeightCm;

            // Render ulang visualisasi titik oranye jika sedang aktif mengedit titik bentuk
            if (isEditingNodes && el.dataset.type === 'shape') {
                renderNodeHandles(el);
            }
            // Render ulang lengkungan teks jika itu teks bebas
            if (el.dataset.type === 'text') {
                applyTextStylesAndWarp(el);
            }
        }
    </script>
</body>
</html>
