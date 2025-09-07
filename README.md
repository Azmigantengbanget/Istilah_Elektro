<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Apa Itu 'Resistor'?", "id": "Komponen Pasif Penghambat Arus Listrik." },
    { "en": "Apa Itu 'Kapasitor'?", "id": "Menyimpan Energi Dalam Medan Listrik." },
    { "en": "Apa Itu 'Induktor'?", "id": "Menyimpan Energi Dalam Medan Magnet." },
    { "en": "Apa Itu 'Transistor'?", "id": "Komponen Semikonduktor Untuk Saklar/Penguat." },
    { "en": "Apa Itu 'Dioda'?", "id": "Menyearahkan Arus Listrik Satu Arah." },
    { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Sirkuit Elektronik Terintegrasi Dalam Chip." },
    { "en": "Apa Itu 'Tegangan' (Voltage)?", "id": "Beda Potensial Antara Dua Titik." },
    { "en": "Apa Itu 'Arus' (Current)?", "id": "Aliran Muatan Listrik Per Waktu." },
    { "en": "Apa Itu 'Hambatan' (Resistance)?", "id": "Ukuran Penolakan Terhadap Arus Listrik." },
    { "en": "Apa Itu 'Daya' (Power)?", "id": "Laju Energi Listrik Yang Ditransfer." },
    { "en": "Apa Itu 'Frekuensi'?", "id": "Jumlah Getaran Gelombang Per Detik." },
    { "en": "Apa Itu 'Amplitudo'?", "id": "Simpangan Maksimum Dari Posisi Setimbang." },
    { "en": "Apa Itu 'Fase'?", "id": "Posisi Relatif Gelombang Pada Waktu." },
    { "en": "Apa Itu 'Osilator'?", "id": "Rangkaian Pembangkit Sinyal Periodik." },
    { "en": "Apa Itu 'Amplifier' (Penguat)?", "id": "Rangkaian Untuk Memperkuat Sinyal Listrik." },
    { "en": "Apa Itu 'Filter' (Penyaring)?", "id": "Melewatkan Frekuensi Tertentu, Menolak Lainnya." },
    { "en": "Apa Itu 'Transformator' (Trafo)?", "id": "Mengubah Level Tegangan AC." },
    { "en": "Apa Itu 'Relay'?", "id": "Saklar Yang Dioperasikan Secara Elektromagnetik." },
    { "en": "Apa Itu 'Sekring' (Fuse)?", "id": "Komponen Pengaman Pemutus Arus Berlebih." },
    { "en": "Apa Itu 'Ground' (Tanah)?", "id": "Titik Referensi Tegangan Nol Volt." },
    { "en": "Apa Itu 'Sirkuit' (Rangkaian)?", "id": "Jalur Tertutup Untuk Aliran Arus." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Untuk Menghubungkan Komponen Elektronik." },
    { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Listrik Serbaguna." },
    { "en": "Apa Itu 'Osiloskop'?", "id": "Menampilkan Bentuk Gelombang Sinyal Listrik." },
    { "en": "Apa Itu 'Generator Sinyal'?", "id": "Alat Pembangkit Berbagai Bentuk Gelombang." },
    { "en": "Apa Itu 'Catu Daya' (Power Supply)?", "id": "Sumber Energi Listrik Untuk Rangkaian." },
    { "en": "Apa Itu 'Arus AC'?", "id": "Arus Listrik Bolak-Balik." },
    { "en": "Apa Itu 'Arus DC'?", "id": "Arus Listrik Searah." },
    { "en": "Apa Itu 'Semikonduktor'?", "id": "Bahan Antara Konduktor Dan Isolator." },
    { "en": "Apa Itu 'Konduktor'?", "id": "Bahan Yang Mudah Menghantarkan Listrik." },
    { "en": "Apa Itu 'Isolator'?", "id": "Bahan Yang Sulit Menghantarkan Listrik." },
    { "en": "Apa Itu 'Gerbang Logika'?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
    { "en": "Apa Itu 'Bit'?", "id": "Unit Informasi Digital Dasar (0/1)." },
    { "en": "Apa Itu 'Byte'?", "id": "Kumpulan Delapan Bit." },
    { "en": "Apa Itu 'Mikrokontroler'?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Itu 'Mikroprosesor'?", "id": "Unit Pemrosesan Pusat (CPU)." },
    { "en": "Apa Itu 'Sensor'?", "id": "Perangkat Pengubah Besaran Fisik/Kimia." },
    { "en": "Apa Itu 'Aktuator'?", "id": "Perangkat Pengubah Sinyal Jadi Gerakan." },
    { "en": "Apa Itu 'Motor Listrik'?", "id": "Mengubah Energi Listrik Jadi Gerak." },
    { "en": "Apa Itu 'Generator Listrik'?", "id": "Mengubah Energi Gerak Jadi Listrik." },
    { "en": "Apa Itu 'Solder'?", "id": "Alat Untuk Melelehkan Timah Patri." },
    { "en": "Apa Itu 'Timah Solder'?", "id": "Logam Untuk Menyambung Kaki Komponen." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Papan Rangkaian Prototipe Tanpa Solder." },
    { "en": "Apa Itu 'Impedansi'?", "id": "Hambatan Total Rangkaian Pada Sinyal AC." },
    { "en": "Apa Itu 'Reaktansi'?", "id": "Hambatan Oleh Induktor Atau Kapasitor." },
    { "en": "Apa Itu 'Admitansi'?", "id": "Kebalikan Dari Impedansi." },
    { "en": "Apa Itu 'Konduktansi'?", "id": "Kebalikan Dari Resistansi." },
    { "en": "Apa Itu 'Resonansi'?", "id": "Getaran Maksimum Pada Frekuensi Tertentu." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
    { "en": "Apa Itu 'Noise' (Derau)?", "id": "Sinyal Listrik Yang Tidak Diinginkan." },
    { "en": "Apa Itu 'Sinyal Analog'?", "id": "Sinyal Kontinu Yang Mewakili Informasi." },
    { "en": "Apa Itu 'Sinyal Digital'?", "id": "Sinyal Diskret Dengan Nilai Terbatas." },
    { "en": "Apa Itu 'Modulasi'?", "id": "Proses Menumpangkan Sinyal Informasi." },
    { "en": "Apa Itu 'Demodulasi'?", "id": "Proses Memisahkan Sinyal Informasi." },
    { "en": "Apa Itu 'Antena'?", "id": "Mengubah Sinyal Listrik Jadi Elektromagnetik." },
    { "en": "Apa Itu 'Transceiver'?", "id": "Perangkat Pemancar Dan Penerima Gabungan." },
    { "en": "Apa Itu 'Transmitter' (Pemancar)?", "id": "Perangkat Yang Mengirimkan Sinyal Radio." },
    { "en": "Apa Itu 'Receiver' (Penerima)?", "id": "Perangkat Yang Menerima Sinyal Radio." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
    { "en": "Apa Itu 'LCD' (Liquid Crystal Display)?", "id": "Tampilan Menggunakan Kristal Cair." },
    { "en": "Apa Itu 'Baterai'?", "id": "Sumber Energi Listrik Kimiawi." },
    { "en": "Apa Itu 'Aki' (Accumulator)?", "id": "Baterai Yang Dapat Diisi Ulang." },
    { "en": "Apa Itu 'Saklar' (Switch)?", "id": "Komponen Untuk Memutus/Menyambung Sirkuit." },
    { "en": "Apa Itu 'Jumper'?", "id": "Kabel Pendek Penghubung Titik Sirkuit." },
    { "en": "Apa Itu 'Kabel'?", "id": "Kawat Konduktor Berbungkus Isolator." },
    { "en": "Apa Itu 'Konektor'?", "id": "Perangkat Untuk Menghubungkan Kabel/Sirkuit." },
    { "en": "Apa Itu 'Terminal'?", "id": "Titik Sambungan Pada Komponen Listrik." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Komputer Digital Untuk Otomasi Industri." },
    { "en": "Apa Itu 'Inverter'?", "id": "Mengubah Tegangan DC Menjadi AC." },
    { "en": "Apa Itu 'Rectifier' (Penyearah)?", "id": "Mengubah Tegangan AC Menjadi DC." },
    { "en": "Apa Itu 'Regulator Tegangan'?", "id": "Menjaga Tegangan Output Tetap Stabil." },
    { "en": "Apa Itu 'Op-Amp' (Operational Amplifier)?", "id": "Penguat Diferensial Tegangan Sangat Tinggi." },
    { "en": "Apa Itu 'Flip-Flop'?", "id": "Rangkaian Penyimpan Satu Bit Data." },
    { "en": "Apa Itu 'Counter' (Pencacah)?", "id": "Rangkaian Yang Menghitung Jumlah Pulsa." },
    { "en": "Apa Itu 'Register'?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
    { "en": "Apa Itu 'ADC' (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu 'DAC' (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Itu 'Solenoida'?", "id": "Kumparan Kawat Pembentuk Medan Magnet." },
    { "en": "Apa Itu 'Elektromagnet'?", "id": "Magnet Yang Dihasilkan Oleh Arus." },
    { "en": "Apa Itu 'Hukum Ohm'?", "id": "Hubungan Antara Tegangan, Arus, Hambatan." },
    { "en": "Apa Itu 'Hukum Kirchhoff I'?", "id": "Total Arus Masuk Sama Dengan Keluar." },
    { "en": "Apa Itu 'Hukum Kirchhoff II'?", "id": "Total Tegangan Dalam Loop Tertutup." },
    { "en": "Apa Itu 'Sirkuit Seri'?", "id": "Komponen Terhubung Dalam Satu Jalur." },
    { "en": "Apa Itu 'Sirkuit Paralel'?", "id": "Komponen Terhubung Secara Berdampingan." },
    { "en": "Apa Itu 'Gelombang Elektromagnetik'?", "id": "Getaran Medan Listrik Dan Magnet." },
    { "en": "Apa Itu 'Spektrum Elektromagnetik'?", "id": "Rentang Semua Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Potensiometer'?", "id": "Resistor Variabel Dengan Tiga Terminal." },
    { "en": "Apa Itu 'Termistor'?", "id": "Resistor Yang Peka Terhadap Suhu." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Resistor Yang Peka Terhadap Cahaya." },
    { "en": "Apa Itu 'Varistor'?", "id": "Resistor Pelindung Dari Lonjakan Tegangan." },
    { "en": "Apa Itu 'Kortsluiting' (Hubung Singkat)?", "id": "Hubungan Arus Listrik Sangat Besar." },
    { "en": "Apa Itu 'Open Circuit' (Rangkaian Terbuka)?", "id": "Sirkuit Dengan Jalur Arus Terputus." },
    { "en": "Apa Itu 'Efisiensi'?", "id": "Rasio Daya Keluaran Terhadap Masukan." },
    { "en": "Apa Itu 'Desibel' (dB)?", "id": "Satuan Logaritmik Untuk Rasio Daya." },
    { "en": "Apa Itu 'Harmonik'?", "id": "Frekuensi Kelipatan Dari Frekuensi Fundamental." },
    { "en": "Apa Itu 'Gain' (Penguatan)?", "id": "Ukuran Kemampuan Penguat Sinyal." },
    { "en": "Apa Itu 'Kutub Utara Magnet'?", "id": "Ujung Magnet Yang Menunjuk Utara." },
    { "en": "Apa Itu 'Kutub Selatan Magnet'?", "id": "Ujung Magnet Yang Menunjuk Selatan." },
    { "en": "Apa Itu 'Garis Gaya Magnet'?", "id": "Garis Imajiner Arah Medan Magnet." },
    { "en": "Apa Itu 'Fluks Magnetik'?", "id": "Jumlah Garis Gaya Medan Magnet." },
    { "en": "Apa Itu 'Induksi Elektromagnetik'?", "id": "Pembangkitan Arus Listrik Oleh Magnet." },
    { "en": "Apa Itu 'Hukum Faraday'?", "id": "GGL Induksi Sebanding Perubahan Fluks." },
    { "en": "Apa Itu 'Hukum Lenz'?", "id": "Arah Arus Induksi Melawan Perubahan." },
    { "en": "Apa Itu 'Arus Eddy'?", "id": "Arus Induksi Berputar Dalam Konduktor." },
    { "en": "Apa Itu 'Transformator Step-Up'?", "id": "Menaikkan Level Tegangan Listrik AC." },
    { "en": "Apa Itu 'Transformator Step-Down'?", "id": "Menurunkan Level Tegangan Listrik AC." },
    { "en": "Apa Itu 'Inti Besi'?", "id": "Bahan Untuk Memperkuat Medan Magnet." },
    { "en": "Apa Itu 'Belitan Primer'?", "id": "Kumparan Masukan Pada Sebuah Trafo." },
    { "en": "Apa Itu 'Belitan Sekunder'?", "id": "Kumparan Keluaran Pada Sebuah Trafo." },
    { "en": "Apa Itu 'BLDC' (Brushless DC Motor)?", "id": "Motor DC Dengan Komutasi Elektronik." },
    { "en": "Apa Itu 'Motor Stepper'?", "id": "Motor Yang Bergerak Dalam Langkah-Langkah." },
    { "en": "Apa Itu 'Motor Servo'?", "id": "Motor Dengan Kontrol Posisi Presisi." },
    { "en": "Apa Itu 'Enkoder' (Encoder)?", "id": "Sensor Untuk Mengukur Posisi Putaran." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Teknik Mengatur Daya Dengan Lebar Pulsa." },
    { "en": "Apa Itu 'Siklus Kerja' (Duty Cycle)?", "id": "Rasio Waktu 'On' Terhadap Periode." },
    { "en": "Apa Itu 'Diode Zener'?", "id": "Menstabilkan Tegangan Pada Level Tertentu." },
    { "en": "Apa Itu 'Diode Schottky'?", "id": "Dioda Dengan Tegangan Maju Rendah." },
    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Dioda Terkendali Untuk Saklar Daya." },
    { "en": "Apa Itu 'TRIAC' (Triode for Alternating Current)?", "id": "Saklar Semikonduktor Untuk Arus AC." },
    { "en": "Apa Itu 'DIAC' (Diode for Alternating Current)?", "id": "Komponen Pemicu Untuk TRIAC." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya Dengan Kecepatan Tinggi." },
    { "en": "Apa Itu 'MOSFET' (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Transistor Efek Medan Untuk Pensaklaran." },
    { "en": "Apa Itu 'JFET' (Junction Field-Effect Transistor)?", "id": "Transistor Efek Medan Tipe Pertemuan." },
    { "en": "Apa Itu 'Fototransistor'?", "id": "Transistor Yang Dikendalikan Oleh Cahaya." },
    { "en": "Apa Itu 'Optocoupler' (Optoisolator)?", "id": "Mengisolasi Sirkuit Menggunakan Cahaya." },
    { "en": "Apa Itu 'Kristal Osilator'?", "id": "Komponen Pembangkit Frekuensi Sangat Stabil." },
    { "en": "Apa Itu 'Sirkuit Tangki' (Tank Circuit)?", "id": "Rangkaian Resonansi LC Paralel." },
    { "en": "Apa Itu 'Attenuator' (Pelemah)?", "id": "Rangkaian Untuk Mengurangi Amplitudo Sinyal." },
    { "en": "Apa Itu 'Mixer' (Pencampur)?", "id": "Menggabungkan Dua Sinyal Frekuensi." },
    { "en": "Apa Itu 'Multiplexer' (MUX)?", "id": "Memilih Satu Dari Banyak Sinyal." },
    { "en": "Apa Itu 'Demultiplexer' (DEMUX)?", "id": "Mengarahkan Satu Sinyal Ke Banyak." },
    { "en": "Apa Itu 'Latch'?", "id": "Elemen Penyimpan Data Dasar Transparan." },
    { "en": "Apa Itu 'ROM' (Read-Only Memory)?", "id": "Memori Yang Hanya Dapat Dibaca." },
    { "en": "Apa Itu 'RAM' (Random-Access Memory)?", "id": "Memori Yang Dapat Dibaca/Ditulis." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable ROM)?", "id": "ROM Yang Dapat Dihapus/Diprogram Listrik." },
    { "en": "Apa Itu 'Flash Memory'?", "id": "Jenis EEPROM Yang Cepat Dihapus." },
    { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Tertanam Dalam Hardware." },
    { "en": "Apa Itu 'BIOS' (Basic Input/Output System)?", "id": "Firmware Untuk Memulai Sistem Komputer." },
    { "en": "Apa Itu 'Driver'?", "id": "Perangkat Lunak Pengendali Perangkat Keras." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin)?", "id": "Komponen Untuk Membuang Panas Berlebih." },
    { "en": "Apa Itu 'Kipas Pendingin'?", "id": "Meningkatkan Aliran Udara Untuk Pendinginan." },
    { "en": "Apa Itu 'Pasta Termal'?", "id": "Meningkatkan Konduksi Panas Antar Permukaan." },
    { "en": "Apa Itu 'SMD' (Surface-Mount Device)?", "id": "Komponen Yang Dipasang Di Permukaan." },
    { "en": "Apa Itu 'Through-Hole'?", "id": "Komponen Dengan Kaki Menembus PCB." },
    { "en": "Apa Itu 'Skematik'?", "id": "Diagram Representasi Simbolik Rangkaian Elektronik." },
    { "en": "Apa Itu 'Layout PCB' (Printed Circuit Board)?", "id": "Desain Tata Letak Komponen/Jalur." },
    { "en": "Apa Itu 'Gerber File'?", "id": "Format File Standar Untuk Manufaktur." },
    { "en": "Apa Itu 'Solder Mask'?", "id": "Lapisan Pelindung Pada Papan PCB." },
    { "en": "Apa Itu 'Silkscreen'?", "id": "Lapisan Tinta Penanda Komponen PCB." },
    { "en": "Apa Itu 'VCC' (Voltage at the Common Collector)?", "id": "Terminal Catu Daya Positif Umum." },
    { "en": "Apa Itu 'VDD' (Voltage Drain Drain)?", "id": "Terminal Catu Daya Positif MOSFET." },
    { "en": "Apa Itu 'VSS' (Voltage Source Source)?", "id": "Terminal Catu Daya Negatif MOSFET." },
    { "en": "Apa Itu 'GND' (Ground)?", "id": "Singkatan Umum Untuk Ground (Tanah)." },
    { "en": "Apa Itu 'Impedansi Input'?", "id": "Hambatan Terukur Pada Terminal Masukan." },
    { "en": "Apa Itu 'Impedansi Output'?", "id": "Hambatan Terukur Pada Terminal Keluaran." },
    { "en": "Apa Itu 'CMRR' (Common-Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode-Sama." },
    { "en": "Apa Itu 'Slew Rate'?", "id": "Laju Perubahan Tegangan Output Maksimum." },
    { "en": "Apa Itu 'Ripple'?", "id": "Variasi AC Kecil Pada Tegangan DC." },
    { "en": "Apa Itu 'Drop-out Voltage'?", "id": "Beda Tegangan Minimal Regulator." },
    { "en": "Apa Itu 'ESD' (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Yang Merusak." },
    { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Gelombang Elektromagnetik Pada Sirkuit." },
    { "en": "Apa Itu 'Faraday Cage' (Sangkar Faraday)?", "id": "Selubung Pelindung Dari Medan Elektromagnetik." },
    { "en": "Apa Itu 'Kabel Koaksial'?", "id": "Kabel Dengan Konduktor Pusat Terisolasi." },
    { "en": "Apa Itu 'Kabel Twisted Pair'?", "id": "Dua Kawat Konduktor Saling Terpilin." },
    { "en": "Apa Itu 'Serat Optik'?", "id": "Media Transmisi Data Berbasis Cahaya." },
    { "en": "Apa Itu 'Galvanometer'?", "id": "Alat Untuk Mendeteksi Arus Kecil." },
    { "en": "Apa Itu 'Jembatan Wheatstone'?", "id": "Rangkaian Untuk Mengukur Hambatan Listrik." },
    { "en": "Apa Itu 'Termokopel'?", "id": "Sensor Suhu Berbasis Efek Termoelektrik." },
    { "en": "Apa Itu 'Piezoelektrik'?", "id": "Bahan Pembangkit Listrik Akibat Tekanan." },
    { "en": "Apa Itu 'Efek Hall'?", "id": "Pembangkitan Tegangan Akibat Medan Magnet." },
    { "en": "Apa Itu 'Superkonduktor'?", "id": "Bahan Dengan Hambatan Listrik Nol." },
    { "en": "Apa Itu 'Arus Bocor' (Leakage Current)?", "id": "Arus Kecil Mengalir Saat Mati." },
    { "en": "Apa Itu 'Tegangan Tembus' (Breakdown Voltage)?", "id": "Tegangan Penyebab Gagalnya Isolator." },
    { "en": "Apa Itu 'Waktu Tunda Propagasi'?", "id": "Waktu Sinyal Merambat Melalui Gerbang." },
    { "en": "Apa Itu 'Fan-out'?", "id": "Jumlah Input Yang Bisa Digerakkan." },
    { "en": "Apa Itu 'Fan-in'?", "id": "Jumlah Input Pada Gerbang Logika." },
    { "en": "Apa Itu 'Logika Positif'?", "id": "Tegangan Tinggi Mewakili Logika '1'." },
    { "en": "Apa Itu 'Logika Negatif'?", "id": "Tegangan Rendah Mewakili Logika '1'." },
    { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Konfigurasi Output Umum Gerbang TTL." },
    { "en": "Apa Itu 'Keluaran Open-Collector'?", "id": "Output Yang Membutuhkan Resistor Pull-up." },
    { "en": "Apa Itu 'Keluaran Tri-State'?", "id": "Output Dengan Tiga Keadaan Logika." },
    { "en": "Apa Itu 'Resistor Pull-up'?", "id": "Memastikan Level Logika Tinggi Default." },
    { "en": "Apa Itu 'Resistor Pull-down'?", "id": "Memastikan Level Logika Rendah Default." },
    { "en": "Apa Itu 'Debouncing'?", "id": "Mengatasi Kontak Bergetar Pada Saklar." },
    { "en": "Apa Itu 'Watchdog Timer'?", "id": "Mereset Sistem Jika Terjadi Kesalahan." },
    { "en": "Apa Itu 'Brown-out Reset' (BOR)?", "id": "Mereset Sistem Saat Tegangan Turun." },
    { "en": "Apa Itu 'Kode Biner'?", "id": "Sistem Bilangan Berbasis Dua (0,1)." },
    { "en": "Apa Itu 'Kode Heksadesimal'?", "id": "Sistem Bilangan Berbasis Enam Belas." },
    { "en": "Apa Itu 'Kode ASCII' (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter Untuk Teks." },
    { "en": "Apa Itu 'FPGA' (Field-Programmable Gate Array)?", "id": "IC Digital Yang Dapat Dikonfigurasi Pengguna." },
    { "en": "Apa Itu 'CPLD' (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Lebih Sederhana." },
    { "en": "Apa Itu 'ASIC' (Application-Specific Integrated Circuit)?", "id": "IC Yang Didesain Untuk Tugas Khusus." },
    { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
    { "en": "Apa Itu 'SoC' (System on a Chip)?", "id": "Seluruh Sistem Dalam Satu Chip." },
    { "en": "Apa Itu 'Verilog'?", "id": "Bahasa Deskripsi Perangkat Keras (HDL)." },
    { "en": "Apa Itu 'VHDL' (VHSIC Hardware Description Language)?", "id": "Bahasa Deskripsi Perangkat Keras Lainnya." },
    { "en": "Apa Itu 'Simulasi Sirkuit'?", "id": "Pemodelan Perilaku Rangkaian Secara Virtual." },
    { "en": "Apa Itu 'SPICE' (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program Simulasi Rangkaian Analog Populer." },
    { "en": "Apa Itu 'Protokol Komunikasi'?", "id": "Aturan Untuk Pertukaran Data Elektronik." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron Antar Perangkat." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron Cepat." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kawat Sederhana." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Jaringan Komunikasi Kuat Untuk Otomotif." },
    { "en": "Apa Itu 'USB' (Universal Serial Bus)?", "id": "Standar Koneksi Periferal Ke Komputer." },
    { "en": "Apa Itu 'Ethernet'?", "id": "Teknologi Jaringan Area Lokal (LAN)." },
    { "en": "Apa Itu 'Wi-Fi'?", "id": "Teknologi Jaringan Nirkabel Lokal." },
    { "en": "Apa Itu 'Bluetooth'?", "id": "Teknologi Nirkabel Jarak Pendek." },
    { "en": "Apa Itu 'RFID' (Radio-Frequency Identification)?", "id": "Identifikasi Objek Menggunakan Gelombang Radio." },
    { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
    { "en": "Apa Itu 'GPS' (Global Positioning System)?", "id": "Sistem Penentuan Posisi Global Satelit." },
    { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Simbol Komunikasi Serial." },
    { "en": "Apa Itu 'Paritas' (Parity)?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
    { "en": "Apa Itu 'Checksum'?", "id": "Nilai Untuk Memverifikasi Integritas Data." },
    { "en": "Apa Itu 'CRC' (Cyclic Redundancy Check)?", "id": "Kode Pendeteksi Kesalahan Transmisi Data." },
    { "en": "Apa Itu 'Handshaking'?", "id": "Proses Negosiasi Aturan Komunikasi." },
    { "en": "Apa Itu 'Buffer'?", "id": "Area Penyimpanan Data Sementara." },
    { "en": "Apa Itu 'Interrupt' (Interupsi)?", "id": "Sinyal Mendesak Untuk Menghentikan Proses." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Akses Memori Langsung Tanpa CPU." },
    { "en": "Apa Itu 'Arsitektur Von Neumann'?", "id": "Data Dan Instruksi Disimpan Bersama." },
    { "en": "Apa Itu 'Arsitektur Harvard'?", "id": "Memori Data Dan Instruksi Terpisah." },
    { "en": "Apa Itu 'Clock Signal' (Sinyal Jam)?", "id": "Sinyal Sinkronisasi Operasi Sirkuit Digital." },
    { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Tumpang Tindih Eksekusi Instruksi." },
    { "en": "Apa Itu 'Cache Memory'?", "id": "Memori Kecil Berkecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Bagian CPU Untuk Operasi Aritmetika." },
    { "en": "Apa Itu 'Sistem Embedded' (Sistem Tertanam)?", "id": "Sistem Komputer Dalam Perangkat Besar." },
    { "en": "Apa Itu 'RTOS' (Real-Time Operating System)?", "id": "Sistem Operasi Dengan Batasan Waktu." },
    { "en": "Apa Itu 'Kapasitansi Parasitik'?", "id": "Kapasitansi Tak Diinginkan Antar Konduktor." },
    { "en": "Apa Itu 'Induktansi Parasitik'?", "id": "Induktansi Tak Diinginkan Pada Konduktor." },
    { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Sinyal Antar Jalur Berdekatan." },
    { "en": "Apa Itu 'Ground Bounce'?", "id": "Fluktuasi Tegangan Pada Jalur Ground." },
    { "en": "Apa Itu 'S-parameter'?", "id": "Parameter Karakterisasi Jaringan Frekuensi Tinggi." },
    { "en": "Apa Itu 'Smith Chart'?", "id": "Grafik Untuk Analisis Impedansi RF." },
    { "en": "Apa Itu 'Saluran Transmisi' (Transmission Line)?", "id": "Struktur Pemandu Gelombang Elektromagnetik." },
    { "en": "Apa Itu 'Impedansi Karakteristik'?", "id": "Impedansi Saluran Transmisi Tak Terhingga." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Ukuran Ketidakcocokan Impedansi." },
    { "en": "Apa Itu 'Return Loss'?", "id": "Ukuran Daya Sinyal Yang Dipantulkan." },
    { "en": "Apa Itu 'Insertion Loss'?", "id": "Kehilangan Daya Sinyal Lewati Perangkat." },
    { "en": "Apa Itu 'Waveguide' (Pemandu Gelombang)?", "id": "Struktur Logam Berongga Pemandu Gelombang." },
    { "en": "Apa Itu 'Antena Dipol'?", "id": "Antena Dasar Dengan Dua Elemen." },
    { "en": "Apa Itu 'Antena Monopol'?", "id": "Antena Dengan Satu Elemen Aktif." },
    { "en": "Apa Itu 'Antena Yagi-Uda'?", "id": "Antena Direktif Dengan Banyak Elemen." },
    { "en": "Apa Itu 'Antena Parabola'?", "id": "Antena Reflektor Berbentuk Mangkuk." },
    { "en": "Apa Itu 'Polarisasi Gelombang'?", "id": "Orientasi Osilasi Medan Listrik Gelombang." },
    { "en": "Apa Itu 'Gain Antena'?", "id": "Kemampuan Antena Memfokuskan Daya." },
    { "en": "Apa Itu 'Directivity' (Keterarahan)?", "id": "Rasio Intensitas Radiasi Arah Puncak." },
    { "en": "Apa Itu 'Pola Radiasi'?", "id": "Representasi Grafis Kekuatan Sinyal Antena." },
    { "en": "Apa Itu 'Beamwidth'?", "id": "Lebar Sudut Berkas Utama Antena." },
    { "en": "Apa Itu 'Radome'?", "id": "Penutup Pelindung Antena Tahan Cuaca." },
    { "en": "Apa Itu 'Filter Pasif'?", "id": "Filter Terbuat Dari Komponen Pasif." },
    { "en": "Apa Itu 'Filter Aktif'?", "id": "Filter Yang Menggunakan Komponen Aktif." },
    { "en": "Apa Itu 'Filter Low-Pass' (LPF)?", "id": "Melewatkan Sinyal Frekuensi Rendah." },
    { "en": "Apa Itu 'Filter High-Pass' (HPF)?", "id": "Melewatkan Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'Filter Band-Pass' (BPF)?", "id": "Melewatkan Sinyal Dalam Pita Frekuensi." },
    { "en": "Apa Itu 'Filter Band-Stop' (BSF)?", "id": "Menolak Sinyal Dalam Pita Frekuensi." },
    { "en": "Apa Itu 'Frekuensi Cutoff'?", "id": "Frekuensi Batas Operasi Sebuah Filter." },
    { "en": "Apa Itu 'Orde Filter'?", "id": "Menentukan Tingkat Ketajaman Respon Filter." },
    { "en": "Apa Itu 'Faktor Q' (Quality Factor)?", "id": "Ukuran Kualitas Rangkaian Resonansi." },
    { "en": "Apa Itu 'Doping' (dalam Semikonduktor)?", "id": "Penambahan Ketidakmurnian Untuk Mengubah Sifat." },
    { "en": "Apa Itu 'Semikonduktor Tipe-N'?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
    { "en": "Apa Itu 'Semikonduktor Tipe-P'?", "id": "Semikonduktor Dengan Kelebihan Lubang (hole)." },
    { "en": "Apa Itu 'PN Junction' (Sambungan PN)?", "id": "Batas Antara Material Tipe-P/N." },
    { "en": "Apa Itu 'Daerah Deplesi'?", "id": "Area Tanpa Pembawa Muatan Bebas." },
    { "en": "Apa Itu 'Bias Maju' (Forward Bias)?", "id": "Menerapkan Tegangan Untuk Menghantarkan Arus." },
    { "en": "Apa Itu 'Bias Mundur' (Reverse Bias)?", "id": "Menerapkan Tegangan Untuk Menghambat Arus." },
    { "en": "Apa Itu 'Avalanche Breakdown'?", "id": "Kerusakan Dioda Akibat Ionisasi Benturan." },
    { "en": "Apa Itu 'Zener Breakdown'?", "id": "Kerusakan Dioda Akibat Medan Listrik." },
    { "en": "Apa Itu 'Fotokonduktivitas'?", "id": "Peningkatan Konduktivitas Bahan Akibat Cahaya." },
    { "en": "Apa Itu 'Sel Surya' (Solar Cell)?", "id": "Mengubah Energi Cahaya Jadi Listrik." },
    { "en": "Apa Itu 'Elektrolisis'?", "id": "Penguraian Senyawa Kimia Oleh Listrik." },
    { "en": "Apa Itu 'Anoda'?", "id": "Elektroda Tempat Terjadinya Reaksi Oksidasi." },
    { "en": "Apa Itu 'Katoda'?", "id": "Elektroda Tempat Terjadinya Reaksi Reduksi." },
    { "en": "Apa Itu 'Elektrolit'?", "id": "Zat Yang Menghantarkan Ion Listrik." },
    { "en": "Apa Itu 'Galvanisasi'?", "id": "Proses Pelapisan Logam Dengan Listrik." },
    { "en": "Apa Itu 'Korosi'?", "id": "Kerusakan Logam Akibat Reaksi Elektokimia." },
    { "en": "Apa Itu 'Sistem Tiga Fasa'?", "id": "Sistem Daya Listrik Tiga Arus." },
    { "en": "Apa Itu 'Hubungan Bintang' (Star/Wye)?", "id": "Koneksi Tiga Fasa Titik Netral." },
    { "en": "Apa Itu 'Hubungan Delta'?", "id": "Koneksi Tiga Fasa Tanpa Netral." },
    { "en": "Apa Itu 'Daya Nyata' (Real Power)?", "id": "Daya Aktual Yang Digunakan Beban." },
    { "en": "Apa Itu 'Daya Reaktif' (Reactive Power)?", "id": "Daya Yang Dibutuhkan Medan Magnet/Listrik." },
    { "en": "Apa Itu 'Daya Semu' (Apparent Power)?", "id": "Kombinasi Vektorial Daya Nyata/Reaktif." },
    { "en": "Apa Itu 'Faktor Daya' (Power Factor)?", "id": "Rasio Daya Nyata Dan Semu." },
    { "en": "Apa Itu 'Koreksi Faktor Daya'?", "id": "Memperbaiki Faktor Daya Mendekati Satu." },
    { "en": "Apa Itu 'Jaringan Listrik' (Power Grid)?", "id": "Jaringan Interkoneksi Pembangkit Dan Konsumen." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Otomatis Ukuran Kecil." },
    { "en": "Apa Itu 'MCCB' (Molded Case Circuit Breaker)?", "id": "Pemutus Sirkuit Dalam Casing Cetakan." },
    { "en": "Apa Itu 'ELCB' (Earth Leakage Circuit Breaker)?", "id": "Pemutus Sirkuit Akibat Bocor Ke Tanah." },
    { "en": "Apa Itu 'RCCB' (Residual Current Circuit Breaker)?", "id": "Pemutus Sirkuit Berdasarkan Arus Sisa." },
    { "en": "Apa Itu 'Sekering' (Fuse)?", "id": "Komponen Pengaman Melebur Saat Arus Berlebih." },
    { "en": "Apa Itu 'Arester' (Arrester)?", "id": "Pelindung Jaringan Listrik Dari Petir." },
    { "en": "Apa Itu 'Grounding' (Pentanahan)?", "id": "Menghubungkan Sistem Listrik Ke Tanah." },
    { "en": "Apa Itu 'Busbar'?", "id": "Batang Konduktor Distribusi Daya Listrik." },
    { "en": "Apa Itu 'Panel Listrik'?", "id": "Pusat Kontrol Dan Distribusi Listrik." },
    { "en": "Apa Itu 'Kontaktor'?", "id": "Saklar Elektromagnetik Untuk Beban Besar." },
    { "en": "Apa Itu 'Overload Relay'?", "id": "Melindungi Motor Dari Beban Berlebih." },
    { "en": "Apa Itu 'Timer' (Pewaktu)?", "id": "Mengontrol Operasi Berdasarkan Waktu." },
    { "en": "Apa Itu 'Push Button' (Tombol Tekan)?", "id": "Saklar Yang Diaktifkan Dengan Menekan." },
    { "en": "Apa Itu 'Saklar Selektor'?", "id": "Saklar Putar Untuk Memilih Mode." },
    { "en": "Apa Itu 'Lampu Indikator'?", "id": "Memberi Tanda Visual Status Operasi." },
    { "en": "Apa Itu 'Inrush Current'?", "id": "Arus Puncak Sesaat Saat Dihidupkan." },
    { "en": "Apa Itu 'Soft Starter'?", "id": "Mengurangi Arus Puncak Saat Motor Dinyalakan." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Mengatur Kecepatan Motor AC." },
    { "en": "Apa Itu 'Encoder Absolut'?", "id": "Encoder Yang Memberi Posisi Unik." },
    { "en": "Apa Itu 'Encoder Inkremental'?", "id": "Encoder Yang Memberi Pulsa Gerakan." },
    { "en": "Apa Itu 'Tacho Generator'?", "id": "Generator Kecil Pengukur Kecepatan Putaran." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol Dan Akuisisi Data." },
    { "en": "Apa Itu 'HMI' (Human-Machine Interface)?", "id": "Antarmuka Grafis Antara Manusia-Mesin." },
    { "en": "Apa Itu 'Fuzzy Logic' (Logika Fuzzy)?", "id": "Logika Yang Menggunakan Derajat Kebenaran." },
    { "en": "Apa Itu 'Jaringan Saraf Tiruan' (ANN)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
    { "en": "Apa Itu 'Algoritma Genetika'?", "id": "Algoritma Pencarian Berbasis Proses Evolusi." },
    { "en": "Apa Itu 'PID Controller' (Proportional-Integral-Derivative)?", "id": "Mekanisme Umpan Balik Kontrol Loop." },
    { "en": "Apa Itu 'Umpan Balik' (Feedback)?", "id": "Sinyal Keluaran Dikembalikan Ke Masukan." },
    { "en": "Apa Itu 'Sistem Loop Terbuka'?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
    { "en": "Apa Itu 'Sistem Loop Tertutup'?", "id": "Sistem Kontrol Dengan Umpan Balik." },
    { "en": "Apa Itu 'Transduser'?", "id": "Mengubah Satu Bentuk Energi Lainnya." },
    { "en": "Apa Itu 'Strain Gauge'?", "id": "Sensor Pengukur Regangan Pada Objek." },
    { "en": "Apa Itu 'Load Cell'?", "id": "Transduser Pengukur Gaya Atau Beban." },
    { "en": "Apa Itu 'Akselerometer'?", "id": "Sensor Yang Mengukur Percepatan Getaran." },
    { "en": "Apa Itu 'Giroskop'?", "id": "Sensor Pengukur Orientasi Dan Kecepatan Sudut." },
    { "en": "Apa Itu 'Sensor Proximity'?", "id": "Mendeteksi Kehadiran Objek Tanpa Sentuhan." },
    { "en": "Apa Itu 'Sensor Ultrasonik'?", "id": "Mengukur Jarak Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'Sensor Inframerah'?", "id": "Mendeteksi Objek Berdasarkan Radiasi Panas." },
    { "en": "Apa Itu 'Magnetometer'?", "id": "Alat Untuk Mengukur Kekuatan Magnet." },
    { "en": "Apa Itu 'Thyristor'?", "id": "Keluarga Saklar Semikonduktor Daya." },
    { "en": "Apa Itu 'GTO' (Gate Turn-Off Thyristor)?", "id": "Thyristor Yang Bisa Dimatikan Gerbangnya." },
    { "en": "Apa Itu 'Kopling Optik'?", "id": "Nama Lain Untuk Optocoupler/Optoisolator." },
    { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pelindung Dari Lonjakan Induktif." },
    { "en": "Apa Itu 'Snubber Circuit'?", "id": "Rangkaian Peredam Lonjakan Tegangan." },
    { "en": "Apa Itu 'Crowbar Circuit'?", "id": "Rangkaian Proteksi Tegangan Berlebih." },
    { "en": "Apa Itu 'Buck Converter'?", "id": "Konverter DC-DC Penurun Tegangan." },
    { "en": "Apa Itu 'Boost Converter'?", "id": "Konverter DC-DC Penaik Tegangan." },
    { "en": "Apa Itu 'Buck-Boost Converter'?", "id": "Konverter DC-DC Penaik/Penurun Tegangan." },
    { "en": "Apa Itu 'Cuk Converter'?", "id": "Jenis Konverter DC-DC Inverting." },
    { "en": "Apa Itu 'Charge Pump'?", "id": "Konverter DC-DC Menggunakan Kapasitor." },
    { "en": "Apa Itu 'LDO' (Low-Dropout Regulator)?", "id": "Regulator Tegangan Dengan Dropout Rendah." },
    { "en": "Apa Itu 'SMPS' (Switch-Mode Power Supply)?", "id": "Catu Daya Dengan Efisiensi Tinggi." },
    { "en": "Apa Itu 'Sinyal Diferensial'?", "id": "Informasi Dikirim Sebagai Selisih Tegangan." },
    { "en": "Apa Itu 'LVDS' (Low-Voltage Differential Signaling)?", "id": "Standar Sinyal Diferensial Kecepatan Tinggi." },
    { "en": "Apa Itu 'Common Mode Noise'?", "id": "Derau Yang Muncul Sama Persis." },
    { "en": "Apa Itu 'Fourier Transform' (Transformasi Fourier)?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensi." },
    { "en": "Apa Itu 'FFT' (Fast Fourier Transform)?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
    { "en": "Apa Itu 'Laplace Transform' (Transformasi Laplace)?", "id": "Analisis Sistem Dalam Domain Frekuensi-s." },
    { "en": "Apa Itu 'Z-Transform' (Transformasi Z)?", "id": "Analisis Sinyal Dan Sistem Diskret." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan)?", "id": "Mengambil Sampel Sinyal Analog Kontinu." },
    { "en": "Apa Itu 'Teorema Nyquist-Shannon'?", "id": "Syarat Frekuensi Sampling Minimal." },
    { "en": "Apa Itu 'Aliasing'?", "id": "Distorsi Akibat Frekuensi Sampling Rendah." },
    { "en": "Apa Itu 'Kuantisasi' (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level." },
    { "en": "Apa Itu 'Error Kuantisasi'?", "id": "Selisih Antara Nilai Asli Sampel." },
    { "en": "Apa Itu 'Sistem Linear'?", "id": "Sistem Yang Memenuhi Prinsip Superposisi." },
    { "en": "Apa Itu 'Sistem Time-Invariant' (LTI)?", "id": "Sistem Yang Responnya Tidak Berubah." },
    { "en": "Apa Itu 'Respon Impuls'?", "id": "Keluaran Sistem Diberi Input Impuls." },
    { "en": "Apa Itu 'Konvolusi'?", "id": "Operasi Matematis Untuk Mencari Output." },
    { "en": "Apa Itu 'Bode Plot'?", "id": "Grafik Respon Frekuensi Sistem." },
    { "en": "Apa Itu 'Nyquist Plot'?", "id": "Grafik Representasi Stabilitas Sistem Kontrol." },
    { "en": "Apa Itu 'Root Locus'?", "id": "Grafik Posisi Pole Loop Tertutup." },
    { "en": "Apa Itu 'Pole' (Kutub)?", "id": "Nilai Frekuensi Membuat Transfer Tak Terhingga." },
    { "en": "Apa Itu 'Zero' (Nol)?", "id": "Nilai Frekuensi Membuat Transfer Nol." },
    { "en": "Apa Itu 'Fungsi Transfer'?", "id": "Rasio Output Terhadap Input." },
    { "en": "Apa Itu 'Stabilitas Sistem'?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
    { "en": "Apa Itu 'Margin Fasa' (Phase Margin)?", "id": "Ukuran Stabilitas Relatif Sistem." },
    { "en": "Apa Itu 'Margin Penguatan' (Gain Margin)?", "id": "Ukuran Stabilitas Relatif Penguatan Sistem." },
    { "en": "Apa Itu 'Decimation'?", "id": "Proses Mengurangi Laju Sampel Sinyal." },
    { "en": "Apa Itu 'Interpolation'?", "id": "Proses Meningkatkan Laju Sampel Sinyal." },
    { "en": "Apa Itu 'FIR Filter' (Finite Impulse Response)?", "id": "Filter Digital Dengan Respon Impuls Terbatas." },
    { "en": "Apa Itu 'IIR Filter' (Infinite Impulse Response)?", "id": "Filter Digital Dengan Respon Impuls Tak Terbatas." },
    { "en": "Apa Itu 'Jitter'?", "id": "Variasi Waktu Kedatangan Sinyal Periodik." },
    { "en": "Apa Itu 'Phase-Locked Loop' (PLL)?", "id": "Sistem Umpan Balik Sinkronisasi Fasa." },
    { "en": "Apa Itu 'VCO' (Voltage-Controlled Oscillator)?", "id": "Osilator Dengan Frekuensi Terkendali Tegangan." },
    { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis Komponen Elektronik." },
    { "en": "Apa Itu 'Breadboarding'?", "id": "Proses Membuat Prototipe Rangkaian." },
    { "en": "Apa Itu 'Debugging'?", "id": "Proses Mencari Dan Memperbaiki Kesalahan." },
    { "en": "Apa Itu 'Reverse Engineering'?", "id": "Menganalisis Produk Untuk Memahami Desain." },
    { "en": "Apa Itu 'Noise Figure'?", "id": "Ukuran Degradasi SNR Oleh Komponen." },
    { "en": "Apa Itu 'SNR' (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Terhadap Derau." },
    { "en": "Apa Itu 'BER' (Bit Error Rate)?", "id": "Tingkat Kesalahan Bit Dalam Transmisi." },
    { "en": "Apa Itu 'Modulasi Amplitudo' (AM)?", "id": "Informasi Dalam Amplitudo Gelombang Pembawa." },
    { "en": "Apa Itu 'Modulasi Frekuensi' (FM)?", "id": "Informasi Dalam Frekuensi Gelombang Pembawa." },
    { "en": "Apa Itu 'Modulasi Fasa' (PM)?", "id": "Informasi Dalam Fasa Gelombang Pembawa." },
    { "en": "Apa Itu 'ASK' (Amplitude-Shift Keying)?", "id": "Modulasi Digital Menggunakan Amplitudo Berbeda." },
    { "en": "Apa Itu 'FSK' (Frequency-Shift Keying)?", "id": "Modulasi Digital Menggunakan Frekuensi Berbeda." },
    { "en": "Apa Itu 'PSK' (Phase-Shift Keying)?", "id": "Modulasi Digital Menggunakan Fasa Berbeda." },
    { "en": "Apa Itu 'QAM' (Quadrature Amplitude Modulation)?", "id": "Modulasi Menggunakan Amplitudo Dan Fasa." },
    { "en": "Apa Itu 'BPSK' (Binary Phase-Shift Keying)?", "id": "PSK Yang Menggunakan Dua Fasa." },
    { "en": "Apa Itu 'QPSK' (Quadrature Phase-Shift Keying)?", "id": "PSK Yang Menggunakan Empat Fasa." },
    { "en": "Apa Itu 'Diagram Konstelasi'?", "id": "Representasi Grafis Skema Modulasi Digital." },
    { "en": "Apa Itu 'OFDM' (Orthogonal Frequency-Division Multiplexing)?", "id": "Teknik Modulasi Untuk Komunikasi Digital." },
    { "en": "Apa Itu 'CDMA' (Code-Division Multiple Access)?", "id": "Metode Akses Kanal Berbasis Kode." },
    { "en": "Apa Itu 'FDMA' (Frequency-Division Multiple Access)?", "id": "Metode Akses Kanal Berbasis Frekuensi." },
    { "en": "Apa Itu 'TDMA' (Time-Division Multiple Access)?", "id": "Metode Akses Kanal Berbasis Waktu." },
    { "en": "Apa Itu 'Spread Spectrum'?", "id": "Teknik Menebarkan Sinyal Ke Frekuensi Luas." },
    { "en": "Apa Itu 'Fading'?", "id": "Fluktuasi Kekuatan Sinyal Radio Diterima." },
    { "en": "Apa Itu 'Multipath Fading'?", "id": "Fading Akibat Pantulan Sinyal Berganda." },
    { "en": "Apa Itu 'Efek Doppler'?", "id": "Pergeseran Frekuensi Akibat Gerakan Relatif." },
    { "en": "Apa Itu 'Inter-Symbol Interference' (ISI)?", "id": "Gangguan Simbol Akibat Distorsi Kanal." },
    { "en": "Apa Itu 'Equalizer'?", "id": "Filter Untuk Memperbaiki Distorsi Transmisi." },
    { "en": "Apa Itu 'Error Correction Code' (ECC)?", "id": "Kode Untuk Mendeteksi Dan Memperbaiki Error." },
    { "en": "Apa Itu 'Kode Hamming'?", "id": "Jenis Kode Koreksi Error Linear." },
    { "en": "Apa Itu 'Kode Reed-Solomon'?", "id": "Kode Koreksi Error Non-Biner Kuat." },
    { "en": "Apa Itu 'Interleaving'?", "id": "Teknik Mengacak Urutan Data." },
    { "en": "Apa Itu 'Source Coding'?", "id": "Proses Mengompresi Data Dari Sumber." },
    { "en": "Apa Itu 'Channel Coding'?", "id": "Penambahan Redundansi Untuk Melawan Noise." },
    { "en": "Apa Itu 'Kapasitas Kanal Shannon'?", "id": "Batas Atas Laju Data Teoretis." },
    { "en": "Apa Itu 'White Noise' (Derau Putih)?", "id": "Derau Dengan Kerapatan Spektral Seragam." },
    { "en": "Apa Itu 'AWGN' (Additive White Gaussian Noise)?", "id": "Model Derau Umum Dalam Komunikasi." },
    { "en": "Apa Itu 'Ground Loop'?", "id": "Arus Tak Diinginkan Antara Dua Titik." },
    { "en": "Apa Itu 'Isolasi Galvanik'?", "id": "Memisahkan Bagian Sirkuit Secara Elektrik." },
    { "en": "Apa Itu 'Dielektrik'?", "id": "Bahan Isolator Listrik Dapat dipolarisasi." },
    { "en": "Apa Itu 'Kekuatan Dielektrik'?", "id": "Medan Listrik Maksimum Tanpa Gagal." },
    { "en": "Apa Itu 'Konstanta Dielektrik'?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
    { "en": "Apa Itu 'Permeabilitas'?", "id": "Kemampuan Bahan Mendukung Pembentukan Magnet." },
    { "en": "Apa Itu 'Permitivitas'?", "id": "Ukuran Perlawanan Pembentukan Medan Listrik." },
    { "en": "Apa Itu 'Histeresis Magnetik'?", "id": "Ketinggalan Magnetisasi Terhadap Medan Magnet." },
    { "en": "Apa Itu 'Saturasi Magnetik'?", "id": "Kondisi Magnetisasi Maksimum Suatu Bahan." },
    { "en": "Apa Itu 'Reluktansi'?", "id": "Perlawanan Bahan Terhadap Fluks Magnetik." },
    { "en": "Apa Itu 'Gaya Gerak Magnet' (MMF)?", "id": "Penyebab Timbulnya Fluks Magnetik." },
    { "en": "Apa Itu 'Hukum Ampere'?", "id": "Hubungan Medan Magnet Dan Arus." },
    { "en": "Apa Itu 'Hukum Biot-Savart'?", "id": "Menghitung Medan Magnet Oleh Arus." },
    { "en": "Apa Itu 'Persamaan Maxwell'?", "id": "Empat Persamaan Dasar Elektromagnetisme." },
    { "en": "Apa Itu 'Arus Perpindahan' (Displacement Current)?", "id": "Arus Akibat Perubahan Medan Listrik." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Kecenderungan Arus AC Mengalir Permukaan." },
    { "en": "Apa Itu 'Proximity Effect'?", "id": "Efek Arus Akibat Konduktor Berdekatan." },
    { "en": "Apa Itu 'Kelvin'?", "id": "Satuan SI Untuk Suhu Termodinamika." },
    { "en": "Apa Itu 'Derau Termal' (Johnson-Nyquist)?", "id": "Derau Akibat Agitasi Termal Elektron." },
    { "en": "Apa Itu 'Shot Noise'?", "id": "Derau Akibat Sifat Diskret Elektron." },
    { "en": "Apa Itu 'Flicker Noise' (1/f Noise)?", "id": "Derau Frekuensi Rendah Dalam Semikonduktor." },
    { "en": "Apa Itu 'Burst Noise'?", "id": "Derau Akibat Cacat Pada Semikonduktor." },
    { "en": "Apa Itu 'Bandgap Voltage Reference'?", "id": "Sumber Referensi Tegangan Sangat Stabil." },
    { "en": "Apa Itu 'Current Mirror' (Cermin Arus)?", "id": "Sirkuit Untuk Menyalin Arus Listrik." },
    { "en": "Apa Itu 'Differential Amplifier'?", "id": "Menguatkan Selisih Antara Dua Sinyal." },
    { "en": "Apa Itu 'Cascode Amplifier'?", "id": "Konfigurasi Penguat Untuk Bandwidth Tinggi." },
    { "en": "Apa Itu 'Darlington Pair'?", "id": "Gabungan Dua Transistor Bipolar." },
    { "en": "Apa Itu 'Sziklai Pair'?", "id": "Pasangan Transistor Komplementer." },
    { "en": "Apa Itu 'Long-Tailed Pair'?", "id": "Dasar Dari Penguat Diferensial." },
    { "en": "Apa Itu 'Active Load'?", "id": "Beban Menggunakan Komponen Aktif." },
    { "en": "Apa Itu 'Class-A Amplifier'?", "id": "Penguat Dengan Bias Selalu Aktif." },
    { "en": "Apa Itu 'Class-B Amplifier'?", "id": "Dua Transistor Bekerja Secara Bergantian." },
    { "en": "Apa Itu 'Class-AB Amplifier'?", "id": "Kompromi Antara Kelas A Dan B." },
    { "en": "Apa Itu 'Class-C Amplifier'?", "id": "Penguat Bekerja Kurang Dari Setengah." },
    { "en": "Apa Itu 'Class-D Amplifier'?", "id": "Penguat Pensaklaran Dengan Efisiensi Tinggi." },
    { "en": "Apa Itu 'Crossover Distortion'?", "id": "Distorsi Pada Penguat Kelas B." },
    { "en": "Apa Itu 'Harmonic Distortion' (THD)?", "id": "Ukuran Distorsi Akibat Munculnya Harmonik." },
    { "en": "Apa Itu 'Intermodulation Distortion' (IMD)?", "id": "Distorsi Akibat Interaksi Sinyal." },
    { "en": "Apa Itu 'Negative Feedback'?", "id": "Mengurangi Gain, Meningkatkan Stabilitas." },
    { "en": "Apa Itu 'Positive Feedback'?", "id": "Meningkatkan Gain, Dapat Menyebabkan Osilasi." },
    { "en": "Apa Itu 'Kriteria Stabilitas Barkhausen'?", "id": "Syarat Terjadinya Osilasi." },
    { "en": "Apa Itu 'Osilator Colpitts'?", "id": "Osilator LC Menggunakan Pembagi Kapasitif." },
    { "en": "Apa Itu 'Osilator Hartley'?", "id": "Osilator LC Menggunakan Pembagi Induktif." },
    { "en": "Apa Itu 'Osilator Clapp'?", "id": "Modifikasi Dari Osilator Colpitts." },
    { "en": "Apa Itu 'Osilator Pierce'?", "id": "Osilator Menggunakan Kristal Dalam Sirkuit." },
    { "en": "Apa Itu 'Osilator Relaksasi'?", "id": "Osilator Berbasis Pengisian/Pengosongan Kapasitor." },
    { "en": "Apa Itu 'Astable Multivibrator'?", "id": "Osilator Persegi Tanpa Keadaan Stabil." },
    { "en": "Apa Itu 'Monostable Multivibrator'?", "id": "Membangkitkan Satu Pulsa Saat Dipicu." },
    { "en": "Apa Itu 'Bistable Multivibrator'?", "id": "Memiliki Dua Keadaan Stabil (Flip-flop)." },
    { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Komparator Dengan Histeresis." },
    { "en": "Apa Itu 'Komparator'?", "id": "Membandingkan Dua Tegangan Atau Arus." },
    { "en": "Apa Itu 'Peak Detector'?", "id": "Sirkuit Yang Menangkap Nilai Puncak." },
    { "en": "Apa Itu 'Sample and Hold Circuit'?", "id": "Mencuplik Dan Menahan Nilai Tegangan." },
    { "en": "Apa Itu 'Logarithmic Amplifier'?", "id": "Penguat Dengan Keluaran Skala Logaritmik." },
    { "en": "Apa Itu 'Instrumentation Amplifier'?", "id": "Penguat Diferensial Presisi Tinggi." },
    { "en": "Apa Itu 'Isolation Amplifier'?", "id": "Penguat Dengan Isolasi Galvanik." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Standar Untuk Menguji Dan Debugging." },
    { "en": "Apa Itu 'Boundary Scan'?", "id": "Metode Pengujian Interkoneksi Pada PCB." },
    { "en": "Apa Itu 'Probe'?", "id": "Alat Penghubung Instrumen Ukur." },
    { "en": "Apa Itu 'Logic Analyzer'?", "id": "Instrumen Untuk Menganalisis Sinyal Digital." },
    { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Instrumen Untuk Menganalisis Spektrum Sinyal." },
    { "en": "Apa Itu 'Vector Network Analyzer' (VNA)?", "id": "Mengukur Parameter Jaringan Frekuensi Radio." },
    { "en": "Apa Itu 'LCR Meter'?", "id": "Alat Ukur Induktansi, Kapasitansi, Resistansi." },
    { "en": "Apa Itu 'Hi-Pot Test' (High Potential)?", "id": "Pengujian Kekuatan Isolasi Tegangan Tinggi." },
    { "en": "Apa Itu 'Daya Tahan' (Endurance)?", "id": "Kemampuan Komponen Bertahan Dalam Waktu." },
    { "en": "Apa Itu 'MTBF' (Mean Time Between Failures)?", "id": "Waktu Rata-rata Antara Kegagalan Sistem." },
    { "en": "Apa Itu 'RoHS' (Restriction of Hazardous Substances)?", "id": "Direktif Pembatasan Bahan Berbahaya." },
    { "en": "Apa Itu 'WEEE' (Waste Electrical and Electronic Equipment)?", "id": "Direktif Penanganan Limbah Elektronik." },
    { "en": "Apa Itu 'CE Marking'?", "id": "Tanda Kesesuaian Produk Standar Eropa." },
    { "en": "Apa Itu 'UL' (Underwriters Laboratories)?", "id": "Organisasi Sertifikasi Keselamatan Produk Global." },
    { "en": "Apa Itu 'FCC' (Federal Communications Commission)?", "id": "Badan Regulasi Komunikasi Di AS." },
    { "en": "Apa Itu 'IEEE' (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Insinyur Teknik Elektro." },
    { "en": "Apa Itu 'Standar IEEE 802.11'?", "id": "Kumpulan Standar Untuk Jaringan Wi-Fi." },
    { "en": "Apa Itu 'Standar IEEE 802.3'?", "id": "Kumpulan Standar Untuk Jaringan Ethernet." },
    { "en": "Apa Itu 'Standar IEEE 802.15'?", "id": "Standar Untuk Jaringan Area Personal." },
    { "en": "Apa Itu 'NEC' (National Electrical Code)?", "id": "Standar Instalasi Listrik Di AS." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Tingkat Proteksi Terhadap Benda Asing." },
    { "en": "Apa Itu 'NEMA Rating'?", "id": "Standar Untuk Selungkup Peralatan Listrik." },
    { "en": "Apa Itu 'Kelas Insulasi'?", "id": "Tingkat Ketahanan Suhu Bahan Insulasi." },
    { "en": "Apa Itu 'AWG' (American Wire Gauge)?", "id": "Standar Ukuran Diameter Kawat." },
    { "en": "Apa Itu 'Ampacity'?", "id": "Kapasitas Arus Maksimum Sebuah Konduktor." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Penurunan Tegangan Sepanjang Konduktor." },
    { "en": "Apa Itu 'Sistem Tenaga Listrik'?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi Daya." },
    { "en": "Apa Itu 'Pembangkit Listrik'?", "id": "Fasilitas Untuk Membangkitkan Energi Listrik." },
    { "en": "Apa Itu 'Transmisi Daya Listrik'?", "id": "Penyaluran Listrik Jarak Jauh." },
    { "en": "Apa Itu 'Distribusi Daya Listrik'?", "id": "Penyaluran Listrik Ke Pengguna Akhir." },
    { "en": "Apa Itu 'Gardu Induk' (Substation)?", "id": "Fasilitas Transformasi Tegangan Jaringan Listrik." },
    { "en": "Apa Itu 'Switchgear'?", "id": "Peralatan Pemutus, Pengontrol, Pelindung Sirkuit." },
    { "en": "Apa Itu 'Isolator Listrik'?", "id": "Bahan Pencegah Aliran Arus Listrik." },
    { "en": "Apa Itu 'Konduktor Telanjang'?", "id": "Kawat Konduktor Tanpa Lapisan Isolasi." },
    { "en": "Apa Itu 'SUTET' (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Jaringan Transmisi Listrik Udara." },
    { "en": "Apa Itu 'Jaringan Distribusi Radial'?", "id": "Jaringan Dengan Satu Jalur Pasokan." },
    { "en": "Apa Itu 'Jaringan Distribusi Cincin' (Ring)?", "id": "Jaringan Dengan Jalur Pasokan Ganda." },
    { "en": "Apa Itu 'Beban Puncak' (Peak Load)?", "id": "Permintaan Listrik Tertinggi Dalam Periode." },
    { "en": "Apa Itu 'Beban Dasar' (Base Load)?", "id": "Permintaan Listrik Minimum Konstan." },
    { "en": "Apa Itu 'Manajemen Beban'?", "id": "Upaya Mengontrol Permintaan Energi Listrik." },
    { "en": "Apa Itu 'Smart Grid'?", "id": "Jaringan Listrik Cerdas Dengan Komunikasi." },
    { "en": "Apa Itu 'Energi Terbarukan'?", "id": "Sumber Energi Dari Proses Alam." },
    { "en": "Apa Itu 'PLTA' (Pembangkit Listrik Tenaga Air)?", "id": "Pembangkit Listrik Menggunakan Tenaga Air." },
    { "en": "Apa Itu 'PLTS' (Pembangkit Listrik Tenaga Surya)?", "id": "Pembangkit Listrik Menggunakan Sinar Matahari." },
    { "en": "Apa Itu 'PLTB' (Pembangkit Listrik Tenaga Bayu)?", "id": "Pembangkit Listrik Menggunakan Tenaga Angin." },
    { "en": "Apa Itu 'PLTN' (Pembangkit Listrik Tenaga Nuklir)?", "id": "Pembangkit Listrik Menggunakan Reaksi Nuklir." },
    { "en": "Apa Itu 'Sel Bahan Bakar' (Fuel Cell)?", "id": "Menghasilkan Listrik Dari Reaksi Kimia." },
    { "en": "Apa Itu 'Co-generation' (Kogenerasi)?", "id": "Produksi Listrik Dan Panas Bersamaan." },
    { "en": "Apa Itu 'Sinkronisasi Jaringan'?", "id": "Menyesuaikan Frekuensi Generator Ke Jaringan." },
    { "en": "Apa Itu 'Blackout' (Padam Listrik Total)?", "id": "Hilangnya Daya Listrik Area Luas." },
    { "en": "Apa Itu 'Brownout'?", "id": "Penurunan Tegangan Jaringan Secara Sengaja." },
    { "en": "Apa Itu 'Stabilitas Transien'?", "id": "Kemampuan Sistem Pulih Dari Gangguan." },
    { "en": "Apa Itu 'Studi Aliran Daya'?", "id": "Analisis Aliran Daya Listrik Sistem." },
    { "en": "Apa Itu 'Analisis Gangguan'?", "id": "Studi Perilaku Sistem Saat Gangguan." },
    { "en": "Apa Itu 'Kalibrasi'?", "id": "Proses Penyetelan Keakuratan Instrumen Ukur." },
    { "en": "Apa Itu 'Presisi'?", "id": "Kedekatan Nilai Pengukuran Satu Sama Lain." },
    { "en": "Apa Itu 'Resolusi'?", "id": "Perubahan Terkecil Yang Dapat Diukur." },
    { "en": "Apa Itu 'Sensitivitas'?", "id": "Rasio Perubahan Output Terhadap Input." },
    { "en": "Apa Itu 'Linearitas'?", "id": "Kesesuaian Kurva Kalibrasi Garis Lurus." },
    { "en": "Apa Itu 'Sistem Satuan Internasional' (SI)?", "id": "Sistem Pengukuran Standar Global Modern." },
    { "en": "Apa Itu 'Volt'?", "id": "Satuan SI Untuk Beda Potensial." },
    { "en": "Apa Itu 'Ampere'?", "id": "Satuan SI Untuk Arus Listrik." },
    { "en": "Apa Itu 'Ohm'?", "id": "Satuan SI Untuk Hambatan Listrik." },
    { "en": "Apa Itu 'Watt'?", "id": "Satuan SI Untuk Daya Listrik." },
    { "en": "Apa Itu 'Joule'?", "id": "Satuan SI Untuk Energi." },
    { "en": "Apa Itu 'Hertz'?", "id": "Satuan SI Untuk Frekuensi." },
    { "en": "Apa Itu 'Farad'?", "id": "Satuan SI Untuk Kapasitansi." },
    { "en": "Apa Itu 'Henry'?", "id": "Satuan SI Untuk Induktansi." },
    { "en": "Apa Itu 'Coulomb'?", "id": "Satuan SI Untuk Muatan Listrik." },
    { "en": "Apa Itu 'Tesla'?", "id": "Satuan SI Untuk Kerapatan Fluks Magnet." },
    { "en": "Apa Itu 'Weber'?", "id": "Satuan SI Untuk Fluks Magnetik." },
    { "en": "Apa Itu 'Siemens'?", "id": "Satuan SI Untuk Konduktansi Listrik." },
    { "en": "Apa Itu 'Lumen'?", "id": "Satuan SI Untuk Fluks Cahaya." },
    { "en": "Apa Itu 'Lux'?", "id": "Satuan SI Untuk Iluminasi." },
    { "en": "Apa Itu 'Candela'?", "id": "Satuan SI Untuk Intensitas Cahaya." },
    { "en": "Apa Itu 'Listrik Statis'?", "id": "Ketidakseimbangan Muatan Listrik Dalam Benda." },
    { "en": "Apa Itu 'Triboelectric Effect'?", "id": "Efek Timbulnya Muatan Saat Bergesekan." },
    { "en": "Apa Itu 'Induksi Elektrostatik'?", "id": "Pemisahan Muatan Akibat Objek Bermuatan." },
    { "en": "Apa Itu 'Sangkar Faraday'?", "id": "Selubung Konduktif Pelindung Medan Listrik." },
    { "en": "Apa Itu 'Generator Van de Graaff'?", "id": "Generator Elektrostatik Tegangan Sangat Tinggi." },
    { "en": "Apa Itu 'Hukum Coulomb'?", "id": "Menghitung Gaya Antara Dua Muatan." },
    { "en": "Apa Itu 'Medan Listrik'?", "id": "Ruang Sekitar Muatan Listrik." },
    { "en": "Apa Itu 'Potensial Listrik'?", "id": "Energi Potensial Per Satuan Muatan." },
    { "en": "Apa Itu 'Garis Ekuipotensial'?", "id": "Garis Dengan Potensial Listrik Sama." },
    { "en": "Apa Itu 'Hukum Gauss'?", "id": "Hubungan Fluks Listrik Dan Muatan." },
    { "en": "Apa Itu 'Kerapatan Muatan'?", "id": "Jumlah Muatan Per Satuan Volume." },
    { "en": "Apa Itu 'Dipol Listrik'?", "id": "Sepasang Muatan Sama Berlawanan Terpisah." },
    { "en": "Apa Itu 'Momen Dipol Listrik'?", "id": "Ukuran Kekuatan Dan Orientasi Dipol." },
    { "en": "Apa Itu 'Elektronika Daya'?", "id": "Aplikasi Elektronika Pada Konversi Daya." },
    { "en": "Apa Itu 'Chopper'?", "id": "Rangkaian Pengubah Daya DC Tetap." },
    { "en": "Apa Itu 'Cycloconverter'?", "id": "Pengubah Frekuensi AC Ke AC Langsung." },
    { "en": "Apa Itu 'Matrix Converter'?", "id": "Konverter AC-AC Tanpa Penyimpanan Energi." },
    { "en": "Apa Itu 'Daya Reaktif Statis' (SVC)?", "id": "Pengatur Cepat Daya Reaktif Jaringan." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "SVC Berbasis Konverter Sumber Tegangan." },
    { "en": "Apa Itu 'FACTS' (Flexible AC Transmission Systems)?", "id": "Sistem Pengontrol Aliran Daya AC." },
    { "en": "Apa Itu 'HVDC' (High-Voltage Direct Current)?", "id": "Sistem Transmisi Daya Arus Searah." },
    { "en": "Apa Itu 'Motor Sinkron'?", "id": "Motor AC Kecepatan Rotor Sinkron." },
    { "en": "Apa Itu 'Motor Asinkron' (Induksi)?", "id": "Motor AC Kecepatan Rotor Asinkron." },
    { "en": "Apa Itu 'Slip' (Dalam Motor Induksi)?", "id": "Perbedaan Kecepatan Rotor Dan Medan." },
    { "en": "Apa Itu 'Stator'?", "id": "Bagian Diam Dari Mesin Listrik." },
    { "en": "Apa Itu 'Rotor'?", "id": "Bagian Berputar Dari Mesin Listrik." },
    { "en": "Apa Itu 'Komutator'?", "id": "Saklar Putar Pembalik Arah Arus." },
    { "en": "Apa Itu 'Sikat' (Brush)?", "id": "Kontak Geser Penghantar Arus Listrik." },
    { "en": "Apa Itu 'Motor Universal'?", "id": "Dapat Beroperasi Dengan Daya AC/DC." },
    { "en": "Apa Itu 'Pengereman Dinamis'?", "id": "Pengereman Mengubah Motor Jadi Generator." },
    { "en": "Apa Itu 'Pengereman Regeneratif'?", "id": "Mengembalikan Energi Pengereman Ke Sumber." },
    { "en": "Apa Itu 'Dielektrik Konstan'?", "id": "Rasio Permitivitas Bahan Terhadap Vakum." },
    { "en": "Apa Itu 'Material Diamagnetik'?", "id": "Bahan Yang Sedikit Menolak Medan Magnet." },
    { "en": "Apa Itu 'Material Paramagnetik'?", "id": "Bahan Yang Sedikit Tertarik Medan Magnet." },
    { "en": "Apa Itu 'Material Feromagnetik'?", "id": "Bahan Yang Sangat Tertarik Medan Magnet." },
    { "en": "Apa Itu 'Suhu Curie'?", "id": "Suhu Hilangnya Sifat Feromagnetik Bahan." },
    { "en": "Apa Itu 'Domain Magnetik'?", "id": "Wilayah Kecil Dengan Arah Magnetisasi Seragam." },
    { "en": "Apa Itu 'Magnetisasi'?", "id": "Kerapatan Momen Magnetik Dalam Suatu Bahan." },
    { "en": "Apa Itu 'Susceptibilitas Magnetik'?", "id": "Ukuran Seberapa Mudah Bahan Dimagnetisasi." },
    { "en": "Apa Itu 'Pita Valensi'?", "id": "Pita Energi Terluar Berisi Elektron." },
    { "en": "Apa Itu 'Pita Konduksi'?", "id": "Pita Energi Tempat Elektron Bergerak Bebas." },
    { "en": "Apa Itu 'Celah Pita' (Band Gap)?", "id": "Energi Pemisah Pita Valensi-Konduksi." },
    { "en": "Apa Itu 'Elektron Valensi'?", "id": "Elektron Pada Kulit Terluar Atom." },
    { "en": "Apa Itu 'Hole' (Lubang)?", "id": "Kekosongan Elektron Dalam Pita Valensi." },
    { "en": "Apa Itu 'Rekombinasi'?", "id": "Proses Elektron Mengisi Sebuah Lubang." },
    { "en": "Apa Itu 'Masa Hidup Pembawa Muatan'?", "id": "Waktu Rata-rata Sebelum Terjadi Rekombinasi." },
    { "en": "Apa Itu 'Mobilitas Pembawa Muatan'?", "id": "Kemudahan Pembawa Muatan Bergerak." },
    { "en": "Apa Itu 'Semikonduktor Intrinsik'?", "id": "Semikonduktor Murni Tanpa Doping." },
    { "en": "Apa Itu 'Semikonduktor Ekstrinsik'?", "id": "Semikonduktor Dengan Penambahan Doping." },
    { "en": "Apa Itu 'Tingkat Fermi'?", "id": "Tingkat Energi Dengan Peluang Terisi 50%." },
    { "en": "Apa Itu 'Arus Difusi'?", "id": "Arus Akibat Gerakan Pembawa Muatan." },
    { "en": "Apa Itu 'Arus Drift'?", "id": "Arus Akibat Medan Listrik Eksternal." },
    { "en": "Apa Itu 'MEMS' (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanik Dan Elektrik Mikroskopis." },
    { "en": "Apa Itu 'Fabrikasi Semikonduktor'?", "id": "Proses Pembuatan Perangkat Semikonduktor." },
    { "en": "Apa Itu 'Wafer Silikon'?", "id": "Irisan Tipis Silikon Dasar IC." },
    { "en": "Apa Itu 'Cleanroom' (Ruang Bersih)?", "id": "Ruangan Sangat Bersih Untuk Fabrikasi." },
    { "en": "Apa Itu 'Fotolitografi'?", "id": "Proses Pencetakan Pola Sirkuit." },
    { "en": "Apa Itu 'Photoresist'?", "id": "Bahan Peka Cahaya Untuk Fotolitografi." },
    { "en": "Apa Itu 'Etsa' (Etching)?", "id": "Proses Menghilangkan Lapisan Material." },
    { "en": "Apa Itu 'Deposisi'?", "id": "Proses Penambahan Lapisan Material Tipis." },
    { "en": "Apa Itu 'Implantasi Ion'?", "id": "Teknik Memasukkan Atom Dopan." },
    { "en": "Apa Itu 'Hukum Moore'?", "id": "Prediksi Jumlah Transistor Berlipat Ganda." },
    { "en": "Apa Itu 'Skala Integrasi' (VLSI)?", "id": "Tingkat Kepadatan Komponen Dalam IC." },
    { "en": "Apa Itu 'Yield' (Hasil)?", "id": "Persentase Chip Fungsional Per Wafer." },
    { "en": "Apa Itu 'Die' (Dalam IC)?", "id": "Potongan Kecil Wafer Berisi Sirkuit." },
    { "en": "Apa Itu 'Packaging' (Pengemasan IC)?", "id": "Proses Melindungi Dan Menghubungkan Die." },
    { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambungkan Die Ke Kaki Kemasan." },
    { "en": "Apa Itu 'Flip-Chip'?", "id": "Metode Pemasangan Die Terbalik." },
    { "en": "Apa Itu 'Ball Grid Array' (BGA)?", "id": "Jenis Kemasan IC Dengan Bola Solder." },
    { "en": "Apa Itu 'Quad Flat Package' (QFP)?", "id": "Jenis Kemasan IC Dengan Kaki Samping." },
    { "en": "Apa Itu 'Dual In-line Package' (DIP)?", "id": "Kemasan IC Dengan Dua Baris Pin." },
    { "en": "Apa Itu 'Sistem Bilangan Biner'?", "id": "Sistem Bilangan Berbasis Dua." },
    { "en": "Apa Itu 'Sistem Bilangan Oktal'?", "id": "Sistem Bilangan Berbasis Delapan." },
    { "en": "Apa Itu 'Sistem Bilangan Desimal'?", "id": "Sistem Bilangan Berbasis Sepuluh." },
    { "en": "Apa Itu 'Sistem Bilangan Heksadesimal'?", "id": "Sistem Bilangan Berbasis Enam Belas." },
    { "en": "Apa Itu 'Komplemen Dua' (Two's Complement)?", "id": "Representasi Bilangan Biner Bertanda." },
    { "en": "Apa Itu 'Floating Point' (Titik Mengambang)?", "id": "Representasi Bilangan Real Dengan Eksponen." },
    { "en": "Apa Itu 'Aljabar Boolean'?", "id": "Aljabar Untuk Menganalisis Sirkuit Logika." },
    { "en": "Apa Itu 'Gerbang AND'?", "id": "Keluaran '1' Jika Semua Masukan '1'." },
    { "en": "Apa Itu 'Gerbang OR'?", "id": "Keluaran '1' Jika Salah Satu '1'." },
    { "en": "Apa Itu 'Gerbang NOT'?", "id": "Membalikkan Nilai Logika Masukan." },
    { "en": "Apa Itu 'Gerbang NAND'?", "id": "Kebalikan Dari Gerbang Logika AND." },
    { "en": "Apa Itu 'Gerbang NOR'?", "id": "Kebalikan Dari Gerbang Logika OR." },
    { "en": "Apa Itu 'Gerbang XOR'?", "id": "Keluaran '1' Jika Masukan Berbeda." },
    { "en": "Apa Itu 'Gerbang XNOR'?", "id": "Keluaran '1' Jika Masukan Sama." },
    { "en": "Apa Itu 'Tabel Kebenaran'?", "id": "Tabel Yang Mendeskripsikan Fungsi Logika." },
    { "en": "Apa Itu 'Peta Karnaugh' (K-Map)?", "id": "Metode Penyederhanaan Ekspresi Logika." },
    { "en": "Apa Itu 'Sirkuit Kombinasional'?", "id": "Keluaran Tergantung Kombinasi Masukan Saat Ini." },
    { "en": "Apa Itu 'Sirkuit Sekuensial'?", "id": "Keluaran Tergantung Masukan Dan Keadaan Sebelumnya." },
    { "en": "Apa Itu 'Encoder'?", "id": "Mengubah Data Menjadi Bentuk Kode." },
    { "en": "Apa Itu 'Decoder'?", "id": "Mengubah Kode Kembali Ke Data Asli." },
    { "en": "Apa Itu 'Full Adder'?", "id": "Menjumlahkan Tiga Bit Masukan." },
    { "en": "Apa Itu 'Half Adder'?", "id": "Menjumlahkan Dua Bit Masukan." },
    { "en": "Apa Itu 'Magnitude Comparator'?", "id": "Membandingkan Besaran Dua Bilangan Biner." },
    { "en": "Apa Itu 'Shift Register' (Register Geser)?", "id": "Register Yang Dapat Menggeser Bit." },
    { "en": "Apa Itu 'Ring Counter'?", "id": "Register Geser Dengan Keluaran Terhubung." },
    { "en": "Apa Itu 'Johnson Counter'?", "id": "Variasi Dari Ring Counter." },
    { "en": "Apa Itu 'Finite State Machine' (FSM)?", "id": "Model Komputasi Untuk Sirkuit Sekuensial." },
    { "en": "Apa Itu 'Keadaan' (State)?", "id": "Informasi Tersimpan Dalam Sirkuit Sekuensial." },
    { "en": "Apa Itu 'Diagram Keadaan' (State Diagram)?", "id": "Representasi Grafis Dari Mesin Keadaan." },
    { "en": "Apa Itu 'Mesin Moore'?", "id": "Keluaran FSM Tergantung Keadaan Saja." },
    { "en": "Apa Itu 'Mesin Mealy'?", "id": "Keluaran FSM Tergantung Keadaan-Masukan." },
    { "en": "Apa Itu 'Race Condition'?", "id": "Kondisi Tak Tentu Akibat Penundaan." },
    { "en": "Apa Itu 'Hazard' (Bahaya)?", "id": "Glitch Tak Diinginkan Pada Keluaran." },
    { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Data Stabil Sebelum Clock." },
    { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Data Stabil Setelah Clock." },
    { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
    { "en": "Apa Itu 'Metastability'?", "id": "Keadaan Tak Stabil Dalam Sirkuit Digital." },
    { "en": "Apa Itu 'Sinkronisasi'?", "id": "Memastikan Operasi Sesuai Sinyal Clock." },
    { "en": "Apa Itu 'Desain Asinkron'?", "id": "Desain Sirkuit Tanpa Sinyal Clock." },
    { "en": "Apa Itu 'Kapasitor Bypass'?", "id": "Menyaring Derau Pada Jalur Catu Daya." },
    { "en": "Apa Itu 'Kapasitor Decoupling'?", "id": "Mengisolasi Derau Antar Bagian Sirkuit." },
    { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Pasif Penekan Derau Frekuensi Tinggi." },
    { "en": "Apa Itu 'Via' (Dalam PCB)?", "id": "Lubang Konduktif Penghubung Antar Lapisan." },
    { "en": "Apa Itu 'Trace' (Jalur PCB)?", "id": "Jalur Tembaga Penghubung Komponen." },
    { "en": "Apa Itu 'Annular Ring'?", "id": "Cincin Tembaga Sekitar Lubang PCB." },
    { "en": "Apa Itu 'DRC' (Design Rule Check)?", "id": "Pemeriksaan Aturan Desain Layout PCB." },
    { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Semua Komponen Dalam Desain." },
    { "en": "Apa Itu 'Assembly Drawing'?", "id": "Gambar Panduan Perakitan Papan Sirkuit." },
    { "en": "Apa Itu 'Reflow Soldering'?", "id": "Metode Solder Untuk Komponen SMD." },
    { "en": "Apa Itu 'Wave Soldering'?", "id": "Metode Solder Untuk Komponen Through-hole." },
    { "en": "Apa Itu 'Solder Paste' (Pasta Solder)?", "id": "Campuran Timah Dan Fluks." },
    { "en": "Apa Itu 'Flux' (Fluks)?", "id": "Bahan Kimia Pembersih Proses Solder." },
    { "en": "Apa Itu 'Conformal Coating'?", "id": "Lapisan Tipis Pelindung Papan Sirkuit." },
    { "en": "Apa Itu 'Potting'?", "id": "Mengisi Selungkup Elektronik Dengan Resin." },
{ "en": "Apa Itu 'Akurasi'?", "id": "Kedekatan Nilai Ukur Dengan Nilai Sebenarnya." },
    { "en": "Apa Itu 'Overvoltage' (Tegangan Berlebih)?", "id": "Tegangan Yang Melebihi Batas Normal." },
    { "en": "Apa Itu 'Undervoltage' (Tegangan Kurang)?", "id": "Tegangan Yang Berada Dibawah Batas Normal." },
    { "en": "Apa Itu 'Transient' (Transien)?", "id": "Lonjakan Tegangan Atau Arus Sesaat." },
    { "en": "Apa Itu 'Surge' (Lonjakan)?", "id": "Kenaikan Tegangan Transien Sangat Cepat." },
    { "en": "Apa Itu 'Sag' (Lengkung)?", "id": "Penurunan Tegangan Sesaat Durasi Pendek." },
    { "en": "Apa Itu 'Swell' (Menggembung)?", "id": "Kenaikan Tegangan Sesaat Durasi Pendek." },
    { "en": "Apa Itu 'Kualitas Daya' (Power Quality)?", "id": "Ukuran Kestabilan Sumber Daya Listrik." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Catu Daya Listrik Darurat Tanpa Jeda." },
    { "en": "Apa Itu 'Mode Online UPS'?", "id": "Beban Selalu Ditenagai Melalui Inverter." },
    { "en": "Apa Itu 'Mode Offline UPS'?", "id": "Beralih Ke Baterai Saat Listrik Padam." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator)?", "id": "Sistem Penstabil Tegangan Listrik Otomatis." },
    { "en": "Apa Itu 'Isolation Transformer'?", "id": "Trafo Untuk Mengisolasi Sirkuit Listrik." },
    { "en": "Apa Itu 'Ferroresonant Transformer'?", "id": "Trafo Yang Mengatur Tegangan Secara Pasif." },
    { "en": "Apa Itu 'Autotransformer'?", "id": "Trafo Dengan Satu Belitan Tunggal." },
    { "en": "Apa Itu 'Tap Changer'?", "id": "Mekanisme Mengubah Rasio Belitan Trafo." },
    { "en": "Apa Itu 'Minyak Trafo'?", "id": "Cairan Isolasi Dan Pendingin Trafo." },
    { "en": "Apa Itu 'Uji Hubung Singkat'?", "id": "Pengujian Untuk Menentukan Parameter Seri." },
    { "en": "Apa Itu 'Uji Rangkaian Terbuka'?", "id": "Pengujian Menentukan Parameter Paralel." },
    { "en": "Apa Itu 'Efek Corona'?", "id": "Pelepasan Muatan Listrik Ke Udara." },
    { "en": "Apa Itu 'Kabel Bawah Tanah'?", "id": "Kabel Transmisi Listrik Dibawah Permukaan." },
    { "en": "Apa Itu 'Isolator Gantung'?", "id": "Isolator Untuk Menahan Konduktor Udara." },
    { "en": "Apa Itu 'Isolator Pin'?", "id": "Isolator Dipasang Diatas Lengan Silang." },
    { "en": "Apa Itu 'Menara Transmisi'?", "id": "Struktur Penyangga Saluran Udara." },
    { "en": "Apa Itu 'Cross-arm' (Lengan Silang)?", "id": "Struktur Horizontal Pada Tiang Listrik." },
    { "en": "Apa Itu 'Sag Konduktor'?", "id": "Lengkungan Konduktor Antara Dua Menara." },
    { "en": "Apa Itu 'Sistem Proteksi Petir'?", "id": "Sistem Pengaman Bangunan Dari Petir." },
    { "en": "Apa Itu 'Batang Franklin'?", "id": "Batang Logam Penangkap Sambaran Petir." },
    { "en": "Apa Itu 'Konduktor Turun'?", "id": "Menyalurkan Arus Petir Ke Tanah." },
    { "en": "Apa Itu 'Elektroda Bumi'?", "id": "Menyebarkan Arus Petir Ke Tanah." },
    { "en": "Apa Itu 'Resistivitas Tanah'?", "id": "Hambatan Tanah Terhadap Arus Listrik." },
    { "en": "Apa Itu 'Tegangan Sentuh' (Touch Voltage)?", "id": "Tegangan Saat Menyentuh Benda Bertegangan." },
    { "en": "Apa Itu 'Tegangan Langkah' (Step Voltage)?", "id": "Beda Potensial Antara Dua Kaki." },
    { "en": "Apa Itu 'Ikatan Ekuipotensial'?", "id": "Menghubungkan Bagian Konduktif Secara Elektrik." },
    { "en": "Apa Itu 'Peralatan Pelindung Diri' (APD)?", "id": "Peralatan Keselamatan Untuk Pekerja Listrik." },
    { "en": "Apa Itu 'Lockout-Tagout' (LOTO)?", "id": "Prosedur Pengamanan Mesin Saat Perbaikan." },
    { "en": "Apa Itu 'Arc Flash'?", "id": "Ledakan Energi Akibat Hubungan Singkat." },
    { "en": "Apa Itu 'Termografi Inframerah'?", "id": "Inspeksi Titik Panas Menggunakan Kamera." },
    { "en": "Apa Itu 'Analisis Getaran'?", "id": "Mendeteksi Masalah Mekanis Mesin Listrik." },
    { "en": "Apa Itu 'Tes Resistansi Isolasi'?", "id": "Mengukur Kualitas Isolasi Peralatan Listrik." },
    { "en": "Apa Itu 'Megohmmeter'?", "id": "Alat Ukur Resistansi Isolasi Tinggi." },
    { "en": "Apa Itu 'Sistem Kendali Terdistribusi' (DCS)?", "id": "Sistem Kontrol Proses Dengan Kontroler Tersebar." },
    { "en": "Apa Itu 'Fieldbus'?", "id": "Jaringan Digital Industri Untuk Instrumen." },
    { "en": "Apa Itu 'HART Protocol'?", "id": "Protokol Komunikasi Hibrida Analog-Digital." },
    { "en": "Apa Itu 'Modbus'?", "id": "Protokol Komunikasi Serial Untuk Otomasi." },
    { "en": "Apa Itu 'Profibus'?", "id": "Standar Fieldbus Untuk Otomasi Industri." },
    { "en": "Apa Itu 'Profinet'?", "id": "Standar Ethernet Industri." },
    { "en": "Apa Itu 'Ladder Logic' (Logika Tangga)?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
    { "en": "Apa Itu 'Function Block Diagram' (FBD)?", "id": "Bahasa Pemrograman Grafis Lainnya." },
    { "en": "Apa Itu 'Sequential Function Chart' (SFC)?", "id": "Bahasa Grafis Untuk Proses Sekuensial." },
    { "en": "Apa Itu 'Structured Text' (ST)?", "id": "Bahasa Pemrograman Teks Mirip Pascal." },
    { "en": "Apa Itu 'Instruction List' (IL)?", "id": "Bahasa Pemrograman Teks Tingkat Rendah." },
    { "en": "Apa Itu 'OPC' (Open Platform Communications)?", "id": "Standar Interoperabilitas Perangkat Lunak Industri." },
    { "en": "Apa Itu 'Telemetry'?", "id": "Pengukuran Dan Pengiriman Data Jarak Jauh." },
    { "en": "Apa Itu 'Akuisisi Data' (DAQ)?", "id": "Proses Mengukur Dan Merekam Sinyal." },
    { "en": "Apa Itu 'Pengkondisian Sinyal'?", "id": "Memproses Sinyal Agar Sesuai." },
    { "en": "Apa Itu 'Common-mode Choke'?", "id": "Induktor Penekan Derau Mode-Sama." },
    { "en": "Apa Itu 'Kabel Berpelindung' (Shielded Cable)?", "id": "Kabel Dengan Lapisan Pelindung EMI." },
    { "en": "Apa Itu 'Ground Plane'?", "id": "Lapisan Tembaga Luas Sebagai Referensi." },
    { "en": "Apa Itu 'Decoupling'?", "id": "Mengisolasi Antar Bagian Sirkuit." },
    { "en": "Apa Itu 'Impedance Matching' (Pencocokan Impedansi)?", "id": "Menyamakan Impedansi Untuk Transfer Daya." },
    { "en": "Apa Itu 'Balun'?", "id": "Mengubah Saluran Seimbang Jadi Tak Seimbang." },
    { "en": "Apa Itu 'Circulator'?", "id": "Perangkat Tiga Port Arah Sirkulasi." },
    { "en": "Apa Itu 'Isolator RF'?", "id": "Memungkinkan Sinyal Lewat Satu Arah." },
    { "en": "Apa Itu 'Directional Coupler'?", "id": "Mencuplik Sebagian Daya Sinyal." },
    { "en": "Apa Itu 'Power Divider' (Pembagi Daya)?", "id": "Membagi Sinyal Menjadi Beberapa Jalur." },
    { "en": "Apa Itu 'Power Combiner' (Penggabung Daya)?", "id": "Menggabungkan Sinyal Dari Banyak Jalur." },
    { "en": "Apa Itu 'Duplexer'?", "id": "Memungkinkan Pemancar-Penerima Berbagi Antena." },
    { "en": "Apa Itu 'Diplexer'?", "id": "Menggabungkan/Memisahkan Sinyal Berbasis Frekuensi." },
    { "en": "Apa Itu 'Superheterodyne Receiver'?", "id": "Penerima Radio Dengan Konversi Frekuensi." },
    { "en": "Apa Itu 'Frekuensi Menengah' (IF)?", "id": "Frekuensi Tetap Hasil Pencampuran." },
    { "en": "Apa Itu 'Image Frequency' (Frekuensi Cermin)?", "id": "Frekuensi Tak Diinginkan Dalam Penerima." },
    { "en": "Apa Itu 'Local Oscillator' (LO)?", "id": "Osilator Internal Untuk Pencampuran Sinyal." },
    { "en": "Apa Itu 'Direct Conversion Receiver'?", "id": "Penerima Radio Tanpa Frekuensi Menengah." },
    { "en": "Apa Itu 'Software-Defined Radio' (SDR)?", "id": "Sistem Radio Dengan Pemrosesan Digital." },
    { "en": "Apa Itu 'Cognitive Radio'?", "id": "Radio Cerdas Yang Dapat Beradaptasi." },
    { "en": "Apa Itu 'MIMO' (Multiple-Input Multiple-Output)?", "id": "Menggunakan Banyak Antena Pemancar-Penerima." },
    { "en": "Apa Itu 'Beamforming'?", "id": "Mengarahkan Sinyal Radio Ke Arah." },
    { "en": "Apa Itu 'Diversity Teknik'?", "id": "Menggunakan Banyak Jalur Untuk Melawan Fading." },
    { "en": "Apa Itu 'Space Diversity'?", "id": "Menggunakan Antena Terpisah Secara Fisik." },
    { "en": "Apa Itu 'Frequency Diversity'?", "id": "Mengirim Sinyal Pada Frekuensi Berbeda." },
    { "en": "Apa Itu 'Time Diversity'?", "id": "Mengirim Ulang Sinyal Pada Waktu Berbeda." },
    { "en": "Apa Itu 'Polarization Diversity'?", "id": "Menggunakan Antena Dengan Polarisasi Berbeda." },
    { "en": "Apa Itu 'Cellular Network' (Jaringan Seluler)?", "id": "Jaringan Radio Terdistribusi Berbasis Sel." },
    { "en": "Apa Itu 'Base Station' (Stasiun Pangkalan)?", "id": "Pusat Komunikasi Dalam Satu Sel." },
    { "en": "Apa Itu 'Handoff' (Handover)?", "id": "Proses Pengalihan Panggilan Antar Sel." },
    { "en": "Apa Itu 'IMSI' (International Mobile Subscriber Identity)?", "id": "Nomor Identifikasi Unik Pelanggan Seluler." },
    { "en": "Apa Itu 'IMEI' (International Mobile Equipment Identity)?", "id": "Nomor Identifikasi Unik Perangkat Seluler." },
    { "en": "Apa Itu 'SIM Card' (Subscriber Identity Module)?", "id": "Kartu Pintar Penyimpan Identitas Pelanggan." },
    { "en": "Apa Itu 'Sistem Satelit'?", "id": "Komunikasi Menggunakan Satelit Buatan." },
    { "en": "Apa Itu 'Transponder Satelit'?", "id": "Penerima-Pemancar Otomatis Di Satelit." },
    { "en": "Apa Itu 'Uplink'?", "id": "Transmisi Sinyal Dari Bumi Ke Satelit." },
    { "en": "Apa Itu 'Downlink'?", "id": "Transmisi Sinyal Dari Satelit Ke Bumi." },
    { "en": "Apa Itu 'Footprint' (Jejak Satelit)?", "id": "Area Jangkauan Sinyal Satelit." },
    { "en": "Apa Itu 'GEO' (Geostationary Earth Orbit)?", "id": "Orbit Sinkron Dengan Rotasi Bumi." },
    { "en": "Apa Itu 'LEO' (Low Earth Orbit)?", "id": "Orbit Rendah Dekat Permukaan Bumi." },
    { "en": "Apa Itu 'MEO' (Medium Earth Orbit)?", "id": "Orbit Menengah Antara LEO Dan GEO." },
    { "en": "Apa Itu 'GPIO' (General-Purpose Input/Output)?", "id": "Pin Digital Serbaguna Pada IC." },
    { "en": "Apa Itu 'Bus' (Dalam Komputer)?", "id": "Jalur Komunikasi Yang Digunakan Bersama." },
    { "en": "Apa Itu 'Bus Alamat'?", "id": "Bus Untuk Menentukan Lokasi Memori." },
    { "en": "Apa Itu 'Bus Data'?", "id": "Bus Untuk Mentransfer Data Aktual." },
    { "en": "Apa Itu 'Bus Kontrol'?", "id": "Bus Untuk Mengirim Sinyal Perintah." },
    { "en": "Apa Itu 'Arbitrasi Bus'?", "id": "Proses Menentukan Master Bus Selanjutnya." },
    { "en": "Apa Itu 'Endianness'?", "id": "Urutan Byte Dalam Kata Digital." },
    { "en": "Apa Itu 'Big-Endian'?", "id": "Byte Paling Signifikan Disimpan Pertama." },
    { "en": "Apa Itu 'Little-Endian'?", "id": "Byte Paling Tidak Signifikan Disimpan Pertama." },
    { "en": "Apa Itu 'Compiler' (Kompilator)?", "id": "Menerjemahkan Kode Sumber Ke Kode Mesin." },
    { "en": "Apa Itu 'Assembler'?", "id": "Menerjemahkan Bahasa Assembly Ke Kode Mesin." },
    { "en": "Apa Itu 'Linker'?", "id": "Menggabungkan File Objek Menjadi Executable." },
    { "en": "Apa Itu 'Debugger'?", "id": "Alat Bantu Untuk Analisis Program." },
    { "en": "Apa Itu 'IDE' (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Perangkat Lunak Terpadu." },
    { "en": "Apa Itu 'SDK' (Software Development Kit)?", "id": "Kumpulan Alat Bantu Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Untuk Berinteraksi Dengan Perangkat Lunak." },
    { "en": "Apa Itu 'Checksum'?", "id": "Nilai Untuk Memverifikasi Integritas Data." },
    { "en": "Apa Itu 'Interrupt Vector Table'?", "id": "Tabel Alamat Rutin Penanganan Interupsi." },
    { "en": "Apa Itu 'ISR' (Interrupt Service Routine)?", "id": "Fungsi Yang Dijalankan Saat Interupsi." },
    { "en": "Apa Itu 'Polling'?", "id": "Memeriksa Status Perangkat Secara Berkala." },
    { "en": "Apa Itu 'Stack' (Tumpukan)?", "id": "Area Memori Untuk Data Sementara." },
    { "en": "Apa Itu 'Heap'?", "id": "Area Memori Untuk Alokasi Dinamis." },
    { "en": "Apa Itu 'Stack Overflow'?", "id": "Kondisi Kehabisan Ruang Memori Stack." },
    { "en": "Apa Itu 'Memory Leak' (Bocor Memori)?", "id": "Memori Yang Tidak Dilepaskan Kembali." },
    { "en": "Apa Itu 'Garbage Collection'?", "id": "Proses Otomatis Membersihkan Memori." },
    { "en": "Apa Itu 'Virtual Memory' (Memori Virtual)?", "id": "Teknik Manajemen Memori Oleh OS." },
    { "en": "Apa Itu 'Paging'?", "id": "Membagi Memori Menjadi Blok Ukuran Tetap." },
    { "en": "Apa Itu 'MMU' (Memory Management Unit)?", "id": "Hardware Pengelola Akses Memori." },
    { "en": "Apa Itu 'Kernel'?", "id": "Inti Dari Sebuah Sistem Operasi." },
    { "en": "Apa Itu 'Bootloader'?", "id": "Program Yang Memuat Sistem Operasi." },
    { "en": "Apa Itu 'Fiber Optic' (Serat Optik)?", "id": "Media Transmisi Data Menggunakan Cahaya." },
    { "en": "Apa Itu 'Inti Serat' (Core)?", "id": "Bagian Tengah Serat Optik." },
    { "en": "Apa Itu 'Cladding'?", "id": "Lapisan Luar Inti Serat Optik." },
    { "en": "Apa Itu 'Indeks Bias'?", "id": "Ukuran Pembelokan Cahaya Dalam Medium." },
    { "en": "Apa Itu 'Total Internal Reflection'?", "id": "Prinsip Dasar Transmisi Cahaya." },
    { "en": "Apa Itu 'Single-Mode Fiber'?", "id": "Serat Optik Untuk Satu Mode Cahaya." },
    { "en": "Apa Itu 'Multi-Mode Fiber'?", "id": "Serat Optik Untuk Banyak Mode Cahaya." },
    { "en": "Apa Itu 'Atenuasi' (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal." },
    { "en": "Apa Itu 'Dispersi'?", "id": "Penyebaran Pulsa Sinyal Dalam Serat." },
    { "en": "Apa Itu 'Fusion Splicing'?", "id": "Menyambung Serat Optik Dengan Pemanasan." },
    { "en": "Apa Itu 'OTDR' (Optical Time-Domain Reflectometer)?", "id": "Instrumen Pengujian Jaringan Serat Optik." },
    { "en": "Apa Itu 'Laser'?", "id": "Sumber Cahaya Koheren." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Sumber Cahaya Inkoheren." },
    { "en": "Apa Itu 'Fotodioda'?", "id": "Sensor Cahaya Semikonduktor." },
    { "en": "Apa Itu 'Avalanche Photodiode' (APD)?", "id": "Fotodioda Dengan Penguatan Internal." },
    { "en": "Apa Itu 'PIN Diode'?", "id": "Dioda Dengan Lapisan Intrinsik Lebar." },
    { "en": "Apa Itu 'Tunnel Diode'?", "id": "Dioda Dengan Resistansi Negatif." },
    { "en": "Apa Itu 'Varactor Diode'?", "id": "Dioda Dengan Kapasitansi Terkendali Tegangan." },
    { "en": "Apa Itu 'Gunn Diode'?", "id": "Dioda Pembangkit Gelombang Mikro." },
    { "en": "Apa Itu 'IMPATT Diode'?", "id": "Dioda Osilator Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu 'Hall Effect Sensor'?", "id": "Sensor Yang Mendeteksi Medan Magnet." },
    { "en": "Apa Itu 'Reed Switch'?", "id": "Saklar Yang Dioperasikan Medan Magnet." },
    { "en": "Apa Itu 'Saklar Merkuri'?", "id": "Saklar Menggunakan Cairan Raksa." },
    { "en": "Apa Itu 'Microswitch'?", "id": "Saklar Jepret Dengan Aksi Cepat." },
    { "en": "Apa Itu 'Saklar Toggle'?", "id": "Saklar Dengan Tuas Penggerak." },
    { "en": "Apa Itu 'Saklar Rocker'?", "id": "Saklar Yang Berjungkat-jungkit." },
    { "en": "Apa Itu 'Saklar Rotary'?", "id": "Saklar Yang Diputar Untuk Memilih." },
    { "en": "Apa Itu 'Keypad'?", "id": "Kumpulan Tombol Dalam Susunan Grid." },
    { "en": "Apa Itu 'Matrix Keypad'?", "id": "Keypad Dengan Pemindaian Baris-Kolom." },
    { "en": "Apa Itu '7-Segment Display'?", "id": "Tampilan Digital Untuk Menampilkan Angka." },
    { "en": "Apa Itu 'Dot Matrix Display'?", "id": "Tampilan Tersusun Dari Matriks Titik." },
    { "en": "Apa Itu 'Buzzer'?", "id": "Komponen Pembangkit Suara Sinyal." },
    { "en": "Apa Itu 'Piezoelectric Buzzer'?", "id": "Buzzer Menggunakan Efek Piezoelektrik." },
    { "en": "Apa Itu 'Loudspeaker' (Pengeras Suara)?", "id": "Mengubah Sinyal Listrik Menjadi Suara." },
    { "en": "Apa Itu 'Mikrofon'?", "id": "Mengubah Gelombang Suara Menjadi Sinyal Listrik." },
    { "en": "Apa Itu 'Codec' (Coder-Decoder)?", "id": "Mengkodekan Dan mendekodekan aliran data." },
    { "en": "Apa Itu 'Latency' (Latensi)?", "id": "Waktu Tunda Dalam Transmisi Data." },
    { "en": "Apa Itu 'Throughput'?", "id": "Laju Transfer Data Efektif." },
    { "en": "Apa Itu 'Topologi Jaringan'?", "id": "Tata Letak Fisik Jaringan Komputer." },
    { "en": "Apa Itu 'Topologi Bus'?", "id": "Semua Perangkat Terhubung Satu Kabel." },
    { "en": "Apa Itu 'Topologi Cincin' (Ring)?", "id": "Setiap Perangkat Terhubung Dua Lainnya." },
    { "en": "Apa Itu 'Topologi Bintang' (Star)?", "id": "Semua Perangkat Terhubung Ke Hub." },
    { "en": "Apa Itu 'Topologi Mesh'?", "id": "Setiap Perangkat Terhubung Banyak Lainnya." },
    { "en": "Apa Itu 'Hub'?", "id": "Perangkat Jaringan Penghubung Banyak Port." },
    { "en": "Apa Itu 'Switch' (Jaringan)?", "id": "Hub Cerdas Yang Meneruskan Paket." },
    { "en": "Apa Itu 'Router'?", "id": "Menghubungkan Jaringan Berbeda." },
    { "en": "Apa Itu 'Bridge'?", "id": "Menghubungkan Dua Segmen Jaringan Sama." },
    { "en": "Apa Itu 'Repeater'?", "id": "Memperkuat Dan Mengirim Ulang Sinyal." },
    { "en": "Apa Itu 'Gateway'?", "id": "Menghubungkan Jaringan Dengan Protokol Berbeda." },
    { "en": "Apa Itu 'Firewall'?", "id": "Sistem Keamanan Jaringan." },
    { "en": "Apa Itu 'Alamat MAC' (Media Access Control)?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
    { "en": "Apa Itu 'Alamat IP' (Internet Protocol)?", "id": "Alamat Logis Perangkat Dalam Jaringan." },
    { "en": "Apa Itu 'Subnet Mask'?", "id": "Membagi Alamat IP Menjadi Jaringan." },
    { "en": "Apa Itu 'DHCP' (Dynamic Host Configuration Protocol)?", "id": "Protokol Pemberi Alamat IP Otomatis." },
    { "en": "Apa Itu 'DNS' (Domain Name System)?", "id": "Sistem Penerjemah Nama Domain." },
    { "en": "Apa Itu 'TCP' (Transmission Control Protocol)?", "id": "Protokol Transportasi Berorientasi Koneksi." },
    { "en": "Apa Itu 'UDP' (User Datagram Protocol)?", "id": "Protokol Transportasi Tanpa Koneksi." },
    { "en": "Apa Itu 'HTTP' (Hypertext Transfer Protocol)?", "id": "Protokol Untuk World Wide Web." },
    { "en": "Apa Itu 'FTP' (File Transfer Protocol)?", "id": "Protokol Untuk Transfer File." },
    { "en": "Apa Itu 'SMTP' (Simple Mail Transfer Protocol)?", "id": "Protokol Untuk Mengirim Email." },
    { "en": "Apa Itu 'POP3' (Post Office Protocol 3)?", "id": "Protokol Untuk Menerima Email." },
    { "en": "Apa Itu 'IMAP' (Internet Message Access Protocol)?", "id": "Protokol Akses Email Modern." },
    { "en": "Apa Itu 'SSH' (Secure Shell)?", "id": "Protokol Jaringan Kriptografi." },
    { "en": "Apa Itu 'SSL/TLS'?", "id": "Protokol Keamanan Lapisan Transportasi." },
    { "en": "Apa Itu 'Telnet'?", "id": "Protokol Login Jarak Jauh." },
    { "en": "Apa Itu 'Ping'?", "id": "Utilitas Jaringan Menguji Jangkauan Host." },
    { "en": "Apa Itu 'Traceroute'?", "id": "Menampilkan Rute Paket Lewati Jaringan." },
    { "en": "Apa Itu 'OSI Model' (Open Systems Interconnection)?", "id": "Model Konseptual Untuk Fungsi Jaringan." },
    { "en": "Apa Itu 'Lapisan Fisik' (Physical Layer)?", "id": "Lapisan Pertama Model OSI (Bit)." },
    { "en": "Apa Itu 'Lapisan Data Link'?", "id": "Lapisan Kedua Model OSI (Frame)." },
    { "en": "Apa Itu 'Lapisan Jaringan' (Network Layer)?", "id": "Lapisan Ketiga Model OSI (Packet)." },
    { "en": "Apa Itu 'Lapisan Transport'?", "id": "Lapisan Keempat Model OSI (Segment)." },
    { "en": "Apa Itu 'Lapisan Sesi' (Session Layer)?", "id": "Lapisan Kelima Model OSI." },
    { "en": "Apa Itu 'Lapisan Presentasi'?", "id": "Lapisan Keenam Model OSI." },
    { "en": "Apa Itu 'Lapisan Aplikasi'?", "id": "Lapisan Ketujuh Model OSI." },
    { "en": "Apa Itu 'Enkapsulasi'?", "id": "Proses Menambahkan Header Ke Data." },
    { "en": "Apa Itu 'Dekapsulasi'?", "id": "Proses Melepaskan Header Dari Data." },
    { "en": "Apa Itu 'PDU' (Protocol Data Unit)?", "id": "Unit Data Tunggal Dalam Lapisan." },
    { "en": "Apa Itu 'MTU' (Maximum Transmission Unit)?", "id": "Ukuran Paket Maksimum Yang Diizinkan." },
    { "en": "Apa Itu 'Fragmentasi'?", "id": "Memecah Paket Besar Menjadi Kecil." },
    { "en": "Apa Itu 'VLAN' (Virtual Local Area Network)?", "id": "Jaringan Logis Pada Perangkat Keras." },
    { "en": "Apa Itu 'Trunking'?", "id": "Membawa Lalu Lintas Banyak VLAN." },
    { "en": "Apa Itu 'NAT' (Network Address Translation)?", "id": "Mengubah Alamat IP Privat Publik." },
    { "en": "Apa Itu 'Port Forwarding'?", "id": "Meneruskan Permintaan Jaringan Ke Host." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Aman Melalui Jaringan Publik." },
    { "en": "Apa Itu 'Proxy Server'?", "id": "Server Perantara Antara Pengguna-Internet." },
    { "en": "Apa Itu 'IoT' (Internet of Things)?", "id": "Jaringan Perangkat Fisik Saling Terhubung." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan Untuk IoT." },
    { "en": "Apa Itu 'CoAP' (Constrained Application Protocol)?", "id": "Protokol Web Untuk Perangkat Terbatas." },
    { "en": "Apa Itu '6LoWPAN'?", "id": "IPv6 Diatas Jaringan Area Personal." },
    { "en": "Apa Itu 'Zigbee'?", "id": "Protokol Komunikasi Nirkabel Hemat Daya." },
    { "en": "Apa Itu 'Z-Wave'?", "id": "Protokol Nirkabel Untuk Otomasi Rumah." },
    { "en": "Apa Itu 'LoRaWAN'?", "id": "Protokol Jaringan Luas Daya Rendah." },
    { "en": "Apa Itu 'Sensor Node'?", "id": "Perangkat Dalam Jaringan Sensor Nirkabel." },
    { "en": "Apa Itu 'Sink Node' (Gateway)?", "id": "Titik Pengumpul Data Jaringan Sensor." },
    { "en": "Apa Itu 'Edge Computing'?", "id": "Komputasi Yang Dilakukan Dekat Sumber." },
    { "en": "Apa Itu 'Cloud Computing'?", "id": "Komputasi Yang Dilakukan Di Server." },
    { "en": "Apa Itu 'Kriptografi'?", "id": "Ilmu Mengamankan Komunikasi." },
    { "en": "Apa Itu 'Enkripsi'?", "id": "Mengubah Data Menjadi Bentuk Kode." },
    { "en": "Apa Itu 'Dekripsi'?", "id": "Mengembalikan Data Terkode Ke Asli." },
    { "en": "Apa Itu 'Kriptografi Simetris'?", "id": "Menggunakan Kunci Sama Untuk Enkripsi/Dekripsi." },
    { "en": "Apa Itu 'Kriptografi Asimetris'?", "id": "Menggunakan Kunci Publik Dan Privat." },
    { "en": "Apa Itu 'AES' (Advanced Encryption Standard)?", "id": "Standar Enkripsi Simetris Global." },
    { "en": "Apa Itu 'DES' (Data Encryption Standard)?", "id": "Standar Enkripsi Simetris Yang Lama." },
    { "en": "Apa Itu 'RSA' (Rivest-Shamir-Adleman)?", "id": "Algoritma Kriptografi Asimetris Populer." },
    { "en": "Apa Itu 'Fungsi Hash'?", "id": "Menghasilkan Nilai Unik Ukuran Tetap." },
    { "en": "Apa Itu 'SHA' (Secure Hash Algorithm)?", "id": "Keluarga Fungsi Hash Kriptografis." },
    { "en": "Apa Itu 'MD5' (Message Digest 5)?", "id": "Fungsi Hash Yang Sudah Usang." },
    { "en": "Apa Itu 'Sertifikat Digital'?", "id": "File Digital Untuk Memverifikasi Identitas." },
    { "en": "Apa Itu 'Otoritas Sertifikat' (CA)?", "id": "Entitas Yang Menerbitkan Sertifikat Digital." },
    { "en": "Apa Itu 'Tanda Tangan Digital'?", "id": "Memastikan Keaslian Dan Integritas Data." },
    { "en": "Apa Itu 'Malware'?", "id": "Perangkat Lunak Yang Dirancang Merusak." },
    { "en": "Apa Itu 'Virus'?", "id": "Malware Yang Mereplikasi Diri." },
    { "en": "Apa Itu 'Worm' (Cacing)?", "id": "Malware Yang Menyebar Lewat Jaringan." },
    { "en": "Apa Itu 'Trojan Horse'?", "id": "Malware Yang Menyamar Program Sah." },
    { "en": "Apa Itu 'Ransomware'?", "id": "Malware Yang Mengunci Data Minta Tebusan." },
    { "en": "Apa Itu 'Spyware'?", "id": "Malware Yang Memata-matai Aktivitas Pengguna." },
    { "en": "Apa Itu 'Adware'?", "id": "Malware Yang Menampilkan Iklan." },
    { "en": "Apa Itu 'Phishing'?", "id": "Upaya Penipuan Mendapatkan Informasi Sensitif." },
    { "en": "Apa Itu 'Serangan DoS' (Denial-of-Service)?", "id": "Membuat Layanan Tidak Tersedia." },
    { "en": "Apa Itu 'Serangan DDoS' (Distributed Denial-of-Service)?", "id": "Serangan DoS Dari Banyak Sumber." },
    { "en": "Apa Itu 'Man-in-the-Middle' (MITM)?", "id": "Serangan Pencegatan Komunikasi." },
    { "en": "Apa Itu 'SQL Injection'?", "id": "Serangan Menyisipkan Kueri SQL Jahat." },
    { "en": "Apa Itu 'Cross-Site Scripting' (XSS)?", "id": "Serangan Menyuntikkan Skrip Sisi Klien." },
    { "en": "Apa Itu 'Elektronika Cetak'?", "id": "Mencetak Sirkuit Elektronik Pada Substrat." },
    { "en": "Apa Itu 'Elektronika Fleksibel'?", "id": "Elektronik Yang Dibangun Di Substrat Lentur." },
    { "en": "Apa Itu 'Wearable Electronics' (Elektronik Sandang)?", "id": "Perangkat Elektronik Yang Dapat Dikenakan." },
    { "en": "Apa Itu 'Spintronics'?", "id": "Memanfaatkan Spin Intrinsik Elektron." },
    { "en": "Apa Itu 'Fotonik'?", "id": "Ilmu Dan Teknologi Berbasis Foton." },
    { "en": "Apa Itu 'Fotonik Silikon'?", "id": "Mengintegrasikan Fotonik Pada Platform Silikon." },
    { "en": "Apa Itu 'Optoelektronik'?", "id": "Studi Perangkat Sumber-Deteksi Cahaya." },
    { "en": "Apa Itu 'Quantum Computing'?", "id": "Komputasi Menggunakan Prinsip Mekanika Kuantum." },
    { "en": "Apa Itu 'Qubit' (Quantum Bit)?", "id": "Unit Dasar Informasi Komputasi Kuantum." },
    { "en": "Apa Itu 'Superposisi'?", "id": "Kemampuan Qubit Ada Di Banyak Keadaan." },
    { "en": "Apa Itu 'Keterkaitan Kuantum' (Entanglement)?", "id": "Keterhubungan Antar Qubit." },
    { "en": "Apa Itu 'Baterai Lithium-Ion'?", "id": "Jenis Baterai Isi Ulang Populer." },
    { "en": "Apa Itu 'Baterai Solid-State'?", "id": "Baterai Menggunakan Elektrolit Padat." },
    { "en": "Apa Itu 'Superkapasitor'?", "id": "Kapasitor Dengan Kepadatan Kapasitansi Tinggi." },
    { "en": "Apa Itu 'Wireless Power Transfer'?", "id": "Transfer Energi Listrik Tanpa Kabel." },
    { "en": "Apa Itu 'Inductive Coupling'?", "id": "Transfer Daya Nirkabel Jarak Dekat." },
    { "en": "Apa Itu 'Magnetic Resonance Coupling'?", "id": "Transfer Daya Nirkabel Jarak Menengah." },
    { "en": "Apa Itu 'Energy Harvesting'?", "id": "Memanen Energi Dari Lingkungan Sekitar." },
    { "en": "Apa Itu 'Metamaterial'?", "id": "Material Buatan Dengan Sifat Unik." },
    { "en": "Apa Itu 'Graphene'?", "id": "Lapisan Tunggal Atom Karbon." },
    { "en": "Apa Itu 'Nanoteknologi'?", "id": "Teknologi Dalam Skala Atomik Molekuler." },
    { "en": "Apa Itu 'CAD' (Computer-Aided Design)?", "id": "Desain Berbantuan Komputer." },
    { "en": "Apa Itu 'CAM' (Computer-Aided Manufacturing)?", "id": "Manufaktur Berbantuan Komputer." },
    { "en": "Apa Itu 'CAE' (Computer-Aided Engineering)?", "id": "Rekayasa Berbantuan Komputer." },
    { "en": "Apa Itu 'EDA' (Electronic Design Automation)?", "id": "Alat Perangkat Lunak Desain Elektronik." },
    { "en": "Apa Itu 'Toleransi Komponen'?", "id": "Variasi Nilai Komponen Yang Diizinkan." },
    { "en": "Apa Itu 'Analisis Monte Carlo'?", "id": "Simulasi Statistik Mempertimbangkan Toleransi." },
    { "en": "Apa Itu 'Worst-Case Analysis'?", "id": "Analisis Perilaku Sirkuit Kondisi Ekstrem." },
    { "en": "Apa Itu 'FMEA' (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Kegagalan Dan Efeknya." },
    { "en": "Apa Itu 'Root Cause Analysis' (RCA)?", "id": "Metodologi Pencarian Akar Penyebab Masalah." },
    { "en": "Apa Itu 'Six Sigma'?", "id": "Metodologi Peningkatan Kualitas Proses." },
    { "en": "Apa Itu 'Lean Manufacturing'?", "id": "Filosofi Manufaktur Minimalisasi Pemborosan." },
    { "en": "Apa Itu 'Just-In-Time' (JIT)?", "id": "Strategi Manajemen Inventaris." },
    { "en": "Apa Itu 'Ergonomi'?", "id": "Studi Interaksi Manusia Dengan Sistem." },
    { "en": "Apa Itu 'Osilator Wien Bridge'?", "id": "Osilator Gelombang Sinus Menggunakan Jembatan Wien." },
    { "en": "Apa Itu 'Osilator Phase-Shift'?", "id": "Osilator Berbasis Pergeseran Fasa." },
    { "en": "Apa Itu 'Multivibrator'?", "id": "Rangkaian Pembangkit Sinyal Tak Sinusoidal." },
    { "en": "Apa Itu 'Timer 555'?", "id": "IC Serbaguna Untuk Pewaktu/Osilator." },
    { "en": "Apa Itu 'Mode Astable 555'?", "id": "Mengkonfigurasi 555 Sebagai Osilator." },
    { "en": "Apa Itu 'Mode Monostable 555'?", "id": "Mengkonfigurasi 555 Sebagai Pewaktu Sekali." },
    { "en": "Apa Itu 'Mode Bistable 555'?", "id": "Mengkonfigurasi 555 Sebagai Flip-flop." },
    { "en": "Apa Itu 'Kapasitor Keramik'?", "id": "Kapasitor Dengan Dielektrik Bahan Keramik." },
    { "en": "Apa Itu 'Kapasitor Elektrolit'?", "id": "Kapasitor Terpolarisasi Dengan Kapasitansi Tinggi." },
    { "en": "Apa Itu 'Kapasitor Tantalum'?", "id": "Jenis Kapasitor Elektrolit Dengan Kinerja Tinggi." },
    { "en": "Apa Itu 'Kapasitor Film'?", "id": "Kapasitor Dengan Dielektrik Film Plastik." },
    { "en": "Apa Itu 'Kapasitor Mika'?", "id": "Kapasitor Sangat Stabil Menggunakan Mika." },
    { "en": "Apa Itu 'Kapasitor Variabel' (Varco)?", "id": "Kapasitor Yang Nilainya Dapat Diubah." },
    { "en": "Apa Itu 'ESR' (Equivalent Series Resistance)?", "id": "Resistansi Seri Internal Sebuah Kapasitor." },
    { "en": "Apa Itu 'ESL' (Equivalent Series Inductance)?", "id": "Induktansi Seri Internal Sebuah Kapasitor." },
    { "en": "Apa Itu 'Faktor Disipasi'?", "id": "Ukuran Ketidakefisienan Sebuah Kapasitor." },
    { "en": "Apa Itu 'Arus Bocor' (Leakage)?", "id": "Arus DC Kecil Melalui Kapasitor." },
    { "en": "Apa Itu 'Resistor Komposisi Karbon'?", "id": "Resistor Terbuat Dari Campuran Karbon." },
    { "en": "Apa Itu 'Resistor Film Karbon'?", "id": "Resistor Dengan Elemen Resistif Film Karbon." },
    { "en": "Apa Itu 'Resistor Film Logam'?", "id": "Resistor Presisi Dengan Film Logam." },
    { "en": "Apa Itu 'Resistor Wirewound'?", "id": "Resistor Daya Tinggi Dari Kawat." },
    { "en": "Apa Itu 'Koefisien Suhu Resistor' (TCR)?", "id": "Perubahan Resistansi Terhadap Perubahan Suhu." },
    { "en": "Apa Itu 'Derau Resistor'?", "id": "Derau Termal Yang Dihasilkan Resistor." },
    { "en": "Apa Itu 'Induktor Inti Udara'?", "id": "Induktor Tanpa Material Inti Magnetik." },
    { "en": "Apa Itu 'Induktor Inti Besi'?", "id": "Induktor Menggunakan Inti Besi." },
    { "en": "Apa Itu 'Induktor Inti Ferit'?", "id": "Induktor Menggunakan Inti Bahan Ferit." },
    { "en": "Apa Itu 'Toroid'?", "id": "Induktor Dengan Inti Berbentuk Donat." },
    { "en": "Apa Itu 'Saturasi Inti'?", "id": "Kondisi Inti Tidak Bisa Magnetisasi Lagi." },
    { "en": "Apa Itu 'Faktor Kualitas' (Q)?", "id": "Rasio Reaktansi Terhadap Resistansi Induktor." },
    { "en": "Apa Itu 'Frekuensi Resonansi Diri' (SRF)?", "id": "Frekuensi Dimana Induktor Beresonansi." },
    { "en": "Apa Itu 'Choke'?", "id": "Induktor Untuk Menghalangi Arus AC." },
    { "en": "Apa Itu 'BJT' (Bipolar Junction Transistor)?", "id": "Transistor Yang Dikendalikan Oleh Arus." },
    { "en": "Apa Itu 'FET' (Field-Effect Transistor)?", "id": "Transistor Yang Dikendalikan Oleh Tegangan." },
    { "en": "Apa Itu 'Transistor NPN'?", "id": "Jenis BJT Dengan Lapisan P-N-P." },
    { "en": "Apa Itu 'Transistor PNP'?", "id": "Jenis BJT Dengan Lapisan N-P-N." },
    { "en": "Apa Itu 'Basis' (Base)?", "id": "Terminal Kontrol Pada Transistor BJT." },
    { "en": "Apa Itu 'Kolektor' (Collector)?", "id": "Terminal Keluaran Utama Transistor BJT." },
    { "en": "Apa Itu 'Emitor' (Emitter)?", "id": "Terminal Tempat Pembawa Muatan Berasal." },
    { "en": "Apa Itu 'Gerbang' (Gate)?", "id": "Terminal Kontrol Pada Transistor FET." },
    { "en": "Apa Itu 'Drain'?", "id": "Terminal Keluaran Utama Transistor FET." },
    { "en": "Apa Itu 'Source'?", "id": "Terminal Asal Pembawa Muatan FET." },
    { "en": "Apa Itu 'Daerah Cut-off'?", "id": "Kondisi Transistor Benar-benar Mati." },
    { "en": "Apa Itu 'Daerah Aktif'?", "id": "Kondisi Transistor Bekerja Sebagai Penguat." },
    { "en": "Apa Itu 'Daerah Saturasi'?", "id": "Kondisi Transistor Benar-benar Hidup." },
    { "en": "Apa Itu 'Beta' (hFE)?", "id": "Penguatan Arus DC Transistor BJT." },
    { "en": "Apa Itu 'Garis Beban' (Load Line)?", "id": "Garis Operasi Transistor Pada Kurva." },
    { "en": "Apa Itu 'Titik Q' (Quiescent Point)?", "id": "Titik Operasi DC Sebuah Transistor." },
    { "en": "Apa Itu 'Biasing'?", "id": "Memberi Tegangan DC Untuk Operasi." },
    { "en": "Apa Itu 'Pembagi Tegangan Biasing'?", "id": "Metode Pembiasan Transistor Paling Umum." },
    { "en": "Apa Itu 'Thermal Runaway'?", "id": "Kondisi Panas Berlebih Merusak Transistor." },
    { "en": "Apa Itu 'Konfigurasi Common Emitter'?", "id": "Konfigurasi Penguat BJT Paling Umum." },
    { "en": "Apa Itu 'Konfigurasi Common Collector'?", "id": "Konfigurasi BJT Sebagai Buffer Tegangan." },
    { "en": "Apa Itu 'Konfigurasi Common Base'?", "id": "Konfigurasi BJT Penguatan Tegangan Tinggi." },
    { "en": "Apa Itu 'Konfigurasi Common Source'?", "id": "Konfigurasi Penguat FET Paling Umum." },
    { "en": "Apa Itu 'Konfigurasi Common Drain'?", "id": "Konfigurasi FET Sebagai Buffer Tegangan." },
    { "en": "Apa Itu 'Konfigurasi Common Gate'?", "id": "Konfigurasi FET Sebagai Buffer Arus." },
    { "en": "Apa Itu 'Efek Miller'?", "id": "Peningkatan Kapasitansi Input Karena Penguatan." },
    { "en": "Apa Itu 'Gain-Bandwidth Product' (GBP)?", "id": "Hasil Kali Gain Dan Bandwidth." },
    { "en": "Apa Itu 'Unijunction Transistor' (UJT)?", "id": "Transistor Tiga Terminal Untuk Pemicuan." },
    { "en": "Apa Itu 'Programmable UJT' (PUT)?", "id": "UJT Yang Titik Puncaknya Diprogram." },
    { "en": "Apa Itu 'Tunneling' Kuantum?", "id": "Partikel Menembus Penghalang Potensial." },
    { "en": "Apa Itu 'Efek Fotolistrik'?", "id": "Pelepasan Elektron Akibat Sinar Foton." },
    { "en": "Apa Itu 'Radiasi Benda Hitam'?", "id": "Radiasi Elektromagnetik Dari Benda Ideal." },
    { "en": "Apa Itu 'Termionik'?", "id": "Emisi Elektron Dari Bahan Dipanaskan." },
    { "en": "Apa Itu 'Tabung Vakum'?", "id": "Komponen Elektronik Aktif Era Awal." },
    { "en": "Apa Itu 'Cathode Ray Tube' (CRT)?", "id": "Tabung Vakum Untuk Menampilkan Gambar." },
    { "en": "Apa Itu 'Magnetron'?", "id": "Tabung Vakum Pembangkit Gelombang Mikro." },
    { "en": "Apa Itu 'Klystron'?", "id": "Tabung Vakum Penguat Frekuensi Tinggi." },
    { "en": "Apa Itu 'Traveling-Wave Tube' (TWT)?", "id": "Penguat Gelombang Mikro Berdaya Tinggi." },
    { "en": "Apa Itu 'Plasma'?", "id": "Keadaan Materi Keempat (Gas Terionisasi)." },
    { "en": "Apa Itu 'Tampilan Plasma'?", "id": "Tampilan Menggunakan Sel Plasma Kecil." },
    { "en": "Apa Itu 'OLED' (Organic Light-Emitting Diode)?", "id": "LED Menggunakan Senyawa Organik." },
    { "en": "Apa Itu 'Quantum Dot' (QD)?", "id": "Partikel Nano Semikonduktor Pemancar Cahaya." },
    { "en": "Apa Itu 'Lampu Pijar'?", "id": "Lampu Menghasilkan Cahaya Dari Filamen." },
    { "en": "Apa Itu 'Lampu Fluorescent' (Lampu Neon)?", "id": "Lampu Menggunakan Pendaran Gas." },
    { "en": "Apa Itu 'Ballast'?", "id": "Komponen Untuk Membatasi Arus Lampu." },
    { "en": "Apa Itu 'Lampu HID' (High-Intensity Discharge)?", "id": "Lampu Busur Listrik Intensitas Tinggi." },
    { "en": "Apa Itu 'Lampu Sodium'?", "id": "Jenis Lampu HID Berwarna Kuning." },
    { "en": "Apa Itu 'Lampu Metal Halide'?", "id": "Jenis Lampu HID Dengan Cahaya Putih." },
    { "en": "Apa Itu 'Efikasi Cahaya'?", "id": "Ukuran Efisiensi Sumber Cahaya." },
    { "en": "Apa Itu 'Indeks Rendering Warna' (CRI)?", "id": "Kemampuan Sumber Cahaya Menampilkan Warna." },
    { "en": "Apa Itu 'Suhu Warna'?", "id": "Karakteristik Tampilan Warna Cahaya." },
    { "en": "Apa Itu 'Iluminasi'?", "id": "Jumlah Fluks Cahaya Per Luas." },
    { "en": "Apa Itu 'Luminansi'?", "id": "Intensitas Cahaya Yang Dipancarkan Permukaan." },
    { "en": "Apa Itu 'Fotometri'?", "id": "Ilmu Pengukuran Cahaya." },
    { "en": "Apa Itu 'Radiometri'?", "id": "Ilmu Pengukuran Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Teori Rangkaian'?", "id": "Studi Tentang Rangkaian Listrik." },
    { "en": "Apa Itu 'Analisis Simpul' (Nodal)?", "id": "Metode Analisis Rangkaian Berbasis Tegangan." },
    { "en": "Apa Itu 'Analisis Mesh'?", "id": "Metode Analisis Berbasis Arus Loop." },
    { "en": "Apa Itu 'Teorema Superposisi'?", "id": "Analisis Rangkaian Linear Dengan Superposisi." },
    { "en": "Apa Itu 'Teorema Thevenin'?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan." },
    { "en": "Apa Itu 'Teorema Norton'?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
    { "en": "Apa Itu 'Teorema Transfer Daya Maksimum'?", "id": "Syarat Transfer Daya Maksimal." },
    { "en": "Apa Itu 'Dualitas' (Dalam Rangkaian)?", "id": "Hubungan Timbal Balik Antar Konsep." },
    { "en": "Apa Itu 'Transformasi Y-Δ' (Wye-Delta)?", "id": "Konversi Antara Konfigurasi Rangkaian Tiga Terminal." },
    { "en": "Apa Itu 'Graph Theory' (Teori Graf)?", "id": "Analisis Rangkaian Menggunakan Simpul Dan Cabang." },
    { "en": "Apa Itu 'Port' (Dalam Teori Rangkaian)?", "id": "Sepasang Terminal Tempat Sinyal Masuk/Keluar." },
    { "en": "Apa Itu 'Jaringan Dua Port'?", "id": "Rangkaian Dengan Satu Port Masukan-Keluaran." },
    { "en": "Apa Itu 'Parameter Z' (Impedansi)?", "id": "Mendeskripsikan Jaringan Dua Port (Tegangan)." },
    { "en": "Apa Itu 'Parameter Y' (Admitansi)?", "id": "Mendeskripsikan Jaringan Dua Port (Arus)." },
    { "en": "Apa Itu 'Parameter H' (Hibrida)?", "id": "Campuran Parameter Z Dan Y." },
    { "en": "Apa Itu 'Parameter ABCD' (Transmisi)?", "id": "Menghubungkan Variabel Masukan Ke Keluaran." },
    { "en": "Apa Itu 'Kondisi Awal'?", "id": "Tegangan Kapasitor/Arus Induktor Sesaat." },
    { "en": "Apa Itu 'Respon Transien'?", "id": "Respon Rangkaian Sesaat Setelah Perubahan." },
    { "en": "Apa Itu 'Respon Keadaan Tunak' (Steady-State)?", "id": "Respon Rangkaian Setelah Waktu Lama." },
    { "en": "Apa Itu 'Respon Natural'?", "id": "Perilaku Rangkaian Tanpa Sumber Eksternal." },
    { "en": "Apa Itu 'Respon Paksa' (Forced Response)?", "id": "Perilaku Rangkaian Akibat Sumber Eksternal." },
    { "en": "Apa Itu 'Konstanta Waktu' (Tau)?", "id": "Ukuran Kecepatan Respon Rangkaian RC/RL." },
    { "en": "Apa Itu 'Rangkaian Orde Pertama'?", "id": "Rangkaian Dengan Satu Elemen Penyimpan Energi." },
    { "en": "Apa Itu 'Rangkaian Orde Kedua'?", "id": "Rangkaian Dengan Dua Elemen Penyimpan Energi." },
    { "en": "Apa Itu 'Faktor Redaman' (Damping Factor)?", "id": "Menentukan Sifat Osilasi Respon." },
    { "en": "Apa Itu 'Overdamped'?", "id": "Respon Lambat Tanpa Osilasi." },
    { "en": "Apa Itu 'Critically Damped'?", "id": "Respon Tercepat Tanpa Osilasi." },
    { "en": "Apa Itu 'Underdamped'?", "id": "Respon Dengan Osilasi Yang Mereda." },
    { "en": "Apa Itu 'Fasor'?", "id": "Representasi Bilangan Kompleks Sinyal Sinusoidal." },
    { "en": "Apa Itu 'Domain Frekuensi'?", "id": "Analisis Rangkaian Berdasarkan Frekuensi." },
    { "en": "Apa Itu 'Daya Kompleks'?", "id": "Representasi Daya AC Dalam Bilangan Kompleks." },
    { "en": "Apa Itu 'Segitiga Daya'?", "id": "Hubungan Grafis Daya Nyata, Reaktif, Semu." },
    { "en": "Apa Itu 'Rangkaian Resonansi Seri'?", "id": "Impedansi Minimum Pada Frekuensi Resonansi." },
    { "en": "Apa Itu 'Rangkaian Resonansi Paralel'?", "id": "Impedansi Maksimum Pada Frekuensi Resonansi." },
    { "en": "Apa Itu 'Sirkuit Magnetik'?", "id": "Analogi Rangkaian Listrik Untuk Medan Magnet." },
    { "en": "Apa Itu 'Gaya Gerak Listrik' (GGL)?", "id": "Energi Per Muatan Oleh Sumber." },
    { "en": "Apa Itu 'GGL Induksi Diri'?", "id": "GGL Akibat Perubahan Arus Induktor." },
    { "en": "Apa Itu 'GGL Induksi Timbal Balik'?", "id": "GGL Akibat Perubahan Arus Induktor Lain." },
    { "en": "Apa Itu 'Induktansi Timbal Balik' (Mutual)?", "id": "Ukuran Kopling Magnetik Antar Induktor." },
    { "en": "Apa Itu 'Koefisien Kopling'?", "id": "Fraksi Fluks Magnetik Yang Terhubung." },
    { "en": "Apa Itu 'Dot Convention'?", "id": "Konvensi Tanda Untuk Induktansi Timbal Balik." },
    { "en": "Apa Itu 'Transformator Ideal'?", "id": "Trafo Tanpa Kerugian Dengan Kopling Sempurna." },
    { "en": "Apa Itu 'Robotika'?", "id": "Cabang Teknik Tentang Desain Robot." },
    { "en": "Apa Itu 'Kinematika Robot'?", "id": "Studi Gerakan Robot Tanpa Mempertimbangkan Gaya." },
    { "en": "Apa Itu 'Dinamika Robot'?", "id": "Studi Gerakan Robot Dengan Mempertimbangkan Gaya." },
    { "en": "Apa Itu 'End-Effector'?", "id": "Alat Di Ujung Lengan Robot." },
    { "en": "Apa Itu 'Derajat Kebebasan' (DOF)?", "id": "Jumlah Gerakan Independen Yang Mungkin." },
    { "en": "Apa Itu 'Ruang Kerja' (Workspace)?", "id": "Area Yang Dapat Dicapai End-Effector." },
    { "en": "Apa Itu 'Visi Komputer' (Computer Vision)?", "id": "Memberi Komputer Kemampuan Untuk 'Melihat'." },
    { "en": "Apa Itu 'Pemrosesan Citra'?", "id": "Memanipulasi Gambar Digital Untuk Analisis." },
    { "en": "Apa Itu 'Segmentasi Citra'?", "id": "Membagi Gambar Menjadi Segmen Bermakna." },
    { "en": "Apa Itu 'Deteksi Tepi'?", "id": "Mengidentifikasi Batas Objek Dalam Gambar." },
    { "en": "Apa Itu 'Pengenalan Pola'?", "id": "Mengidentifikasi Pola Dalam Data." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Algoritma Yang Belajar Dari Data." },
    { "en": "Apa Itu 'Deep Learning'?", "id": "Sub-bidang Pembelajaran Mesin." },
    { "en": "Apa Itu 'Teknik Biomedis'?", "id": "Aplikasi Teknik Pada Kedokteran Biologi." },
    { "en": "Apa Itu 'Biosensor'?", "id": "Sensor Yang Menggunakan Elemen Biologis." },
    { "en": "Apa Itu 'Sinyal Biologis'?", "id": "Sinyal Listrik Yang Dihasilkan Tubuh." },
    { "en": "Apa Itu 'EKG/ECG' (Elektrokardiogram)?", "id": "Merekam Aktivitas Listrik Jantung." },
    { "en": "Apa Itu 'EEG' (Electroencephalogram)?", "id": "Merekam Aktivitas Listrik Otak." },
    { "en": "Apa Itu 'EMG' (Elektromiogram)?", "id": "Merekam Aktivitas Listrik Otot." },
    { "en": "Apa Itu 'Pencitraan Medis'?", "id": "Teknik Untuk Melihat Bagian Dalam Tubuh." },
    { "en": "Apa Itu 'Sinar-X'?", "id": "Pencitraan Menggunakan Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'CT Scan' (Computed Tomography)?", "id": "Pencitraan Sinar-X Lintas Penampang." },
    { "en": "Apa Itu 'MRI' (Magnetic Resonance Imaging)?", "id": "Pencitraan Menggunakan Medan Magnet Kuat." },
    { "en": "Apa Itu 'Ultrasound'?", "id": "Pencitraan Menggunakan Gelombang Suara Tinggi." },
    { "en": "Apa Itu 'Implan Medis'?", "id": "Perangkat Buatan Yang Ditanam Tubuh." },
    { "en": "Apa Itu 'Alat Pacu Jantung'?", "id": "Implan Untuk Mengatur Irama Jantung." },
    { "en": "Apa Itu 'Defibrillator'?", "id": "Memberi Kejutan Listrik Ke Jantung." },
    { "en": "Apa Itu 'Implan Koklea'?", "id": "Membantu Mengembalikan Fungsi Pendengaran." },
    { "en": "Apa Itu 'Prostetik'?", "id": "Alat Bantu Pengganti Bagian Tubuh." },
    { "en": "Apa Itu 'Telemedicine'?", "id": "Pemberian Layanan Kesehatan Jarak Jauh." },
    { "en": "Apa Itu 'Bioinformatika'?", "id": "Menggunakan Komputasi Untuk Analisis Data Biologis." },
    { "en": "Apa Itu 'Resistor Shunt'?", "id": "Resistor Paralel Mengalihkan Arus." },
    { "en": "Apa Itu 'Ammeter'?", "id": "Alat Untuk Mengukur Arus Listrik." },
    { "en": "Apa Itu 'Voltmeter'?", "id": "Alat Untuk Mengukur Tegangan Listrik." },
    { "en": "Apa Itu 'Ohmmeter'?", "id": "Alat Untuk Mengukur Hambatan Listrik." },
    { "en": "Apa Itu 'Wattmeter'?", "id": "Alat Untuk Mengukur Daya Listrik." },
    { "en": "Apa Itu 'Pengukur Energi' (kWh meter)?", "id": "Mengukur Konsumsi Energi Listrik." },
    { "en": "Apa Itu 'Jembatan Maxwell'?", "id": "Mengukur Induktansi Tidak Diketahui." },
    { "en": "Apa Itu 'Jembatan Hay'?", "id": "Mengukur Induktansi Dengan Faktor Q Tinggi." },
    { "en": "Apa Itu 'Jembatan Schering'?", "id": "Mengukur Kapasitansi Dan Faktor Disipasi." },
    { "en": "Apa Itu 'Jembatan Wien'?", "id": "Mengukur Frekuensi Sebuah Sinyal." },
    { "en": "Apa Itu 'Current Transformer' (CT)?", "id": "Trafo Untuk Mengukur Arus Tinggi." },
    { "en": "Apa Itu 'Potential Transformer' (PT)?", "id": "Trafo Untuk Mengukur Tegangan Tinggi." },
    { "en": "Apa Itu 'Galvanometer D'Arsonval'?", "id": "Dasar Dari Alat Ukur Analog." },
    { "en": "Apa Itu 'Efek Piroelektrik'?", "id": "Kemampuan Bahan Hasilkan Tegangan Suhu." },
    { "en": "Apa Itu 'Efek Seebeck'?", "id": "Tegangan Timbul Akibat Perbedaan Suhu." },
    { "en": "Apa Itu 'Efek Peltier'?", "id": "Pemanasan/Pendinginan Sambungan Akibat Arus." },
    { "en": "Apa Itu 'Efek Thomson'?", "id": "Pemanasan/Pendinginan Konduktor Akibat Arus." },
    { "en": "Apa Itu 'Pendingin Termoelektrik' (TEC)?", "id": "Perangkat Menggunakan Efek Peltier." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Resistansi Logam." },
    { "en": "Apa Itu 'Hygrometer'?", "id": "Alat Ukur Kelembaban Udara." },
    { "en": "Apa Itu 'Anemometer'?", "id": "Alat Ukur Kecepatan Angin." },
    { "en": "Apa Itu 'Manometer'?", "id": "Alat Ukur Tekanan Fluida." },
    { "en": "Apa Itu 'Flowmeter' (Pengukur Aliran)?", "id": "Mengukur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Level Sensor' (Sensor Ketinggian)?", "id": "Mendeteksi Ketinggian Cairan Atau Padatan." },
    { "en": "Apa Itu 'pH Meter'?", "id": "Mengukur Tingkat Keasaman Suatu Larutan." },
    { "en": "Apa Itu 'Konduktivitas Meter'?", "id": "Mengukur Kemampuan Larutan Hantarkan Listrik." },
    { "en": "Apa Itu 'Spectrometer'?", "id": "Alat Ukur Properti Cahaya." },
    { "en": "Apa Itu 'Kromatografi Gas'?", "id": "Teknik Analisis Pemisahan Komponen Kimia." },
    { "en": "Apa Itu 'Sistem Kontrol Umpan Maju'?", "id": "Sistem Kontrol Prediktif Tanpa Output." },
    { "en": "Apa Itu 'Kontrol Adaptif'?", "id": "Kontroler Yang Menyesuaikan Parameternya." },
    { "en": "Apa Itu 'Kontrol Kuat' (Robust Control)?", "id": "Kontroler Yang Tahan Ketidakpastian." },
    { "en": "Apa Itu 'Kontrol Optimal'?", "id": "Menemukan Sinyal Kontrol Terbaik." },
    { "en": "Apa Itu 'Kontrol Non-linear'?", "id": "Teknik Kontrol Untuk Sistem Non-linear." },
    { "en": "Apa Itu 'Kontrol Prediktif Model' (MPC)?", "id": "Kontroler Menggunakan Model Untuk Memprediksi." },
    { "en": "Apa Itu 'Estimator Keadaan' (State Estimator)?", "id": "Memperkirakan Keadaan Internal Sistem." },
    { "en": "Apa Itu 'Filter Kalman'?", "id": "Algoritma Estimator Optimal Untuk Sistem Linear." },
    { "en": "Apa Itu 'Observer' (Pengamat)?", "id": "Model Dinamis Untuk Mengestimasi Keadaan." },
    { "en": "Apa Itu 'Identifikasi Sistem'?", "id": "Membangun Model Matematis Dari Data." },
    { "en": "Apa Itu 'Ruang Keadaan' (State Space)?", "id": "Representasi Matematis Sistem Dinamis." },
    { "en": "Apa Itu 'Controllability' (Keterkendalian)?", "id": "Kemampuan Mengarahkan Sistem Ke Keadaan Apapun." },
    { "en": "Apa Itu 'Observability' (Keteramatan)?", "id": "Kemampuan Menentukan Keadaan Dari Output." },
    { "en": "Apa Itu 'Eigenvalue' (Nilai Eigen)?", "id": "Menentukan Stabilitas Sistem Ruang Keadaan." },
    { "en": "Apa Itu 'Eigenvector' (Vektor Eigen)?", "id": "Vektor Yang Arahnya Tidak Berubah." },
    { "en": "Apa Itu 'Matriks'?", "id": "Susunan Angka Dalam Baris Dan Kolom." },
    { "en": "Apa Itu 'Determinan Matriks'?", "id": "Nilai Skalar Yang Dihitung Dari Matriks." },
    { "en": "Apa Itu 'Invers Matriks'?", "id": "Matriks Kebalikan Dari Matriks Asli." },
    { "en": "Apa Itu 'Bilangan Kompleks'?", "id": "Bilangan Dengan Bagian Real Imajiner." },
    { "en": "Apa Itu 'Bidang Kompleks'?", "id": "Representasi Grafis Bilangan Kompleks." },
    { "en": "Apa Itu 'Rumus Euler'?", "id": "Menghubungkan Fungsi Trigonometri Eksponensial Kompleks." },
    { "en": "Apa Itu 'Deret Fourier'?", "id": "Menguraikan Fungsi Periodik Jadi Sinusoida." },
    { "en": "Apa Itu 'Sinyal Waktu Kontinu'?", "id": "Sinyal Yang Didefinisikan Setiap Saat." },
    { "en": "Apa Itu 'Sinyal Waktu Diskrit'?", "id": "Sinyal Yang Didefinisikan Waktu Tertentu." },
    { "en": "Apa Itu 'Sinyal Periodik'?", "id": "Sinyal Yang Berulang Dalam Interval." },
    { "en": "Apa Itu 'Sinyal Aperiodik'?", "id": "Sinyal Yang Tidak Berulang." },
    { "en": "Apa Itu 'Sinyal Deterministik'?", "id": "Sinyal Yang Dapat Diprediksi Sempurna." },
    { "en": "Apa Itu 'Sinyal Acak' (Stokastik)?", "id": "Sinyal Dengan Elemen Ketidakpastian." },
    { "en": "Apa Itu 'Fungsi Step Satuan'?", "id": "Sinyal Bernilai Nol Lalu Satu." },
    { "en": "Apa Itu 'Fungsi Impuls Satuan'?", "id": "Sinyal Dengan Durasi Tak Terhingga Singkat." },
    { "en": "Apa Itu 'Fungsi Ramp Satuan'?", "id": "Sinyal Yang Meningkat Secara Linear." },
    { "en": "Apa Itu 'Energi Sinyal'?", "id": "Ukuran Total Energi Sinyal." },
    { "en": "Apa Itu 'Daya Sinyal'?", "id": "Ukuran Daya Rata-rata Sinyal." },
    { "en": "Apa Itu 'Korelasi Silang' (Cross-correlation)?", "id": "Ukuran Kemiripan Dua Sinyal." },
    { "en": "Apa Itu 'Autokorelasi'?", "id": "Korelasi Silang Sinyal Dengan Dirinya." },
    { "en": "Apa Itu 'Kerapatan Spektral Daya' (PSD)?", "id": "Distribusi Daya Sinyal Per Frekuensi." },
    { "en": "Apa Itu 'Proses Ergodik'?", "id": "Proses Acak Statistiknya Konstan." },
    { "en": "Apa Itu 'Proses Stasioner'?", "id": "Proses Acak Distribusinya Tidak Berubah." },
    { "en": "Apa Itu 'Kabel Pita' (Ribbon Cable)?", "id": "Kabel Dengan Banyak Kawat Paralel." },
    { "en": "Apa Itu 'Konektor D-subminiature'?", "id": "Konektor Dengan Pelindung Logam Bentuk-D." },
    { "en": "Apa Itu 'Konektor BNC'?", "id": "Konektor RF Tipe Bayonet Cepat." },
    { "en": "Apa Itu 'Konektor SMA'?", "id": "Konektor RF Ulir Untuk Frekuensi Tinggi." },
    { "en": "Apa Itu 'Konektor RJ45'?", "id": "Konektor Standar Untuk Kabel Ethernet." },
    { "en": "Apa Itu 'Konektor Molex'?", "id": "Konektor Daya Umum Dalam Komputer." },
    { "en": "Apa Itu 'Crimp'?", "id": "Menyambung Kabel Ke Terminal Logam." },
    { "en": "Apa Itu 'Wire Stripper'?", "id": "Alat Untuk Mengupas Isolasi Kabel." },
    { "en": "Apa Itu 'Heat Shrink Tubing'?", "id": "Tabung Plastik Menyusut Saat Dipanaskan." },
    { "en": "Apa Itu 'Terminal Block'?", "id": "Blok Terminal Untuk Menghubungkan Kabel." },
    { "en": "Apa Itu 'DIN Rail'?", "id": "Rel Logam Standar Pemasangan Peralatan." },
    { "en": "Apa Itu 'Enclosure' (Selungkup)?", "id": "Wadah Pelindung Untuk Peralatan Elektronik." },
    { "en": "Apa Itu 'Gasket'?", "id": "Segel Mekanis Untuk Mencegah Kebocoran." },
    { "en": "Apa Itu 'Standoff'?", "id": "Pemisah Berulir Untuk Memasang PCB." },
    { "en": "Apa Itu 'Grommet'?", "id": "Cincin Pelindung Lubang Lewat Kabel." },
    { "en": "Apa Itu 'Cable Tie' (Pengikat Kabel)?", "id": "Tali Plastik Untuk Mengikat Kabel." },
    { "en": "Apa Itu 'Lugs'?", "id": "Konektor Terminal Untuk Kabel Besar." },
    { "en": "Apa Itu 'Busur Listrik' (Electric Arc)?", "id": "Pelepasan Listrik Melalui Gas." },
    { "en": "Apa Itu 'Pengelasan Busur' (Arc Welding)?", "id": "Proses Pengelasan Menggunakan Busur Listrik." },
    { "en": "Apa Itu 'Tungku Busur Listrik'?", "id": "Tungku Peleburan Menggunakan Busur Listrik." },
    { "en": "Apa Itu 'Pemanasan Induksi'?", "id": "Memanaskan Benda Konduktif Secara Elektromagnetik." },
    { "en": "Apa Itu 'Pemanasan Dielektrik'?", "id": "Memanaskan Bahan Isolator Dengan RF." },
    { "en": "Apa Itu 'Oven Microwave'?", "id": "Memanaskan Makanan Dengan Radiasi Mikro." },
    { "en": "Apa Itu 'Elektroplating'?", "id": "Proses Pelapisan Logam Dengan Elektrolisis." },
    { "en": "Apa Itu 'Anodizing'?", "id": "Proses Pelapisan Oksida Pelindung." },
    { "en": "Apa Itu 'Pemesinan Pelepasan Listrik' (EDM)?", "id": "Pemesinan Logam Dengan Erosi Listrik." },
    { "en": "Apa Itu 'Motor Linear'?", "id": "Motor Yang Menghasilkan Gerakan Lurus." },
    { "en": "Apa Itu 'Maglev' (Magnetic Levitation)?", "id": "Mengangkat Objek Menggunakan Gaya Magnet." },
    { "en": "Apa Itu 'Bantalan Magnetik'?", "id": "Bantalan Yang Mendukung Beban Magnet." },
    { "en": "Apa Itu 'Sistem Rem Eddy Current'?", "id": "Pengereman Menggunakan Prinsip Arus Eddy." },
    { "en": "Apa Itu 'Reluktansi Motor'?", "id": "Motor Sinkron Tanpa Belitan Rotor." },
    { "en": "Apa Itu 'Motor Histeresis'?", "id": "Motor Sinkron Menggunakan Kerugian Histeresis." },
    { "en": "Apa Itu 'Pembangkit MHD' (Magnetohydrodynamic)?", "id": "Membangkitkan Listrik Dari Fluida Konduktif." },
    { "en": "Apa Itu 'Sel Efek Hall'?", "id": "Transduser Berbasis Prinsip Efek Hall." },
    { "en": "Apa Itu 'Cryogenics'?", "id": "Studi Suhu Sangat Rendah." },
    { "en": "Apa Itu 'SQUID' (Superconducting Quantum Interference Device)?", "id": "Magnetometer Sangat Sensitif." },
    { "en": "Apa Itu 'Josephson Junction'?", "id": "Efek Kuantum Pada Superkonduktor." },
    { "en": "Apa Itu 'Akselerator Partikel'?", "id": "Mesin Mempercepat Partikel Bermuatan." },
    { "en": "Apa Itu 'Siklotron'?", "id": "Jenis Akselerator Partikel." },
    { "en": "Apa Itu 'Sinkrotron'?", "id": "Jenis Akselerator Partikel Melingkar Besar." },
    { "en": "Apa Itu 'Reaktor Fusi'?", "id": "Perangkat Menghasilkan Energi Fusi Nuklir." },
    { "en": "Apa Itu 'Tokamak'?", "id": "Desain Reaktor Fusi Magnetik." },
    { "en": "Apa Itu 'Laser Fusi'?", "id": "Fusi Yang Dipicu Oleh Laser." },
    { "en": "Apa Itu 'Perlindungan Radiasi'?", "id": "Proteksi Terhadap Radiasi Pengion." },
    { "en": "Apa Itu 'Dosimeter'?", "id": "Alat Pengukur Paparan Dosis Radiasi." },
    { "en": "Apa Itu 'Pencacah Geiger'?", "id": "Detektor Partikel Radiasi Pengion." },
    { "en": "Apa Itu 'Kamar Kabut' (Cloud Chamber)?", "id": "Detektor Untuk Melihat Jejak Partikel." },
    { "en": "Apa Itu 'Kamar Gelembung' (Bubble Chamber)?", "id": "Detektor Jejak Partikel Cairan Panas." },
    { "en": "Apa Itu 'Detektor Sintilasi'?", "id": "Detektor Radiasi Menggunakan Bahan Pendar." },
    { "en": "Apa Itu 'Photomultiplier Tube' (PMT)?", "id": "Tabung Pengganda Foton Sangat Sensitif." },
    { "en": "Apa Itu 'Detektor Semikonduktor'?", "id": "Detektor Radiasi Berbasis Semikonduktor." },
    { "en": "Apa Itu 'Spektroskopi Gamma'?", "id": "Studi Spektrum Energi Sinar Gamma." },
    { "en": "Apa Itu 'Neutron Activation Analysis' (NAA)?", "id": "Teknik Analisis Kimia Sensitif." },
    { "en": "Apa Itu 'Penanggalan Radiokarbon'?", "id": "Metode Penentuan Usia Berbasis Karbon-14." },
    { "en": "Apa Itu 'Efek Cherenkov'?", "id": "Radiasi Elektromagnetik Dari Partikel." },
    { "en": "Apa Itu 'Radiasi Sinkrotron'?", "id": "Radiasi Elektromagnetik Partikel Dipercepat." },
    { "en": "Apa Itu 'Bremsstrahlung'?", "id": "Radiasi Pengereman Partikel Bermuatan." },
    { "en": "Apa Itu 'Hamburan Compton'?", "id": "Hamburan Foton Oleh Partikel Bermuatan." },
    { "en": "Apa Itu 'Produksi Pasangan'?", "id": "Penciptaan Partikel-Antipartikel Dari Foton." },
    { "en": "Apa Itu 'Pemusnahan' (Annihilation)?", "id": "Tabrakan Partikel-Antipartikel Hasilkan Energi." },
    { "en": "Apa Itu 'Model Standar Fisika Partikel'?", "id": "Teori Yang Mendeskripsikan Partikel Fundamental." },
    { "en": "Apa Itu 'Quark'?", "id": "Partikel Elementer Penyusun Hadron." },
    { "en": "Apa Itu 'Lepton'?", "id": "Partikel Elementer (Contohnya Elektron)." },
    { "en": "Apa Itu 'Boson'?", "id": "Partikel Pembawa Gaya Fundamental." },
    { "en": "Apa Itu 'Foton'?", "id": "Boson Pembawa Gaya Elektromagnetik." },
    { "en": "Apa Itu 'Gluon'?", "id": "Boson Pembawa Gaya Nuklir Kuat." },
    { "en": "Apa Itu 'Boson W dan Z'?", "id": "Boson Pembawa Gaya Nuklir Lemah." },
    { "en": "Apa Itu 'Graviton'?", "id": "Boson Hipotetis Pembawa Gaya Gravitasi." },
    { "en": "Apa Itu 'Boson Higgs'?", "id": "Partikel Yang Memberikan Massa Partikel Lain." },
    { "en": "Apa Itu 'Antipartikel'?", "id": "Partikel Dengan Massa Sama Muatan Berlawanan." },
    { "en": "Apa Itu 'Positron'?", "id": "Antipartikel Dari Elektron." },
    { "en": "Apa Itu 'Neutrino'?", "id": "Partikel Netral Dengan Massa Sangat Kecil." },
    { "en": "Apa Itu 'Mekanika Kuantum'?", "id": "Teori Fisika Untuk Skala Atomik." },
    { "en": "Apa Itu 'Prinsip Ketidakpastian Heisenberg'?", "id": "Batas Akurasi Pengukuran Pasangan Variabel." },
    { "en": "Apa Itu 'Dualitas Gelombang-Partikel'?", "id": "Partikel Menunjukkan Sifat Gelombang Partikel." },
    { "en": "Apa Itu 'Fungsi Gelombang'?", "id": "Deskripsi Matematis Keadaan Kuantum." },
    { "en": "Apa Itu 'Persamaan Schrödinger'?", "id": "Mendeskripsikan Evolusi Fungsi Gelombang." },
    { "en": "Apa Itu 'Keadaan Dasar' (Ground State)?", "id": "Tingkat Energi Terendah Sistem Kuantum." },
    { "en": "Apa Itu 'Keadaan Tereksitasi'?", "id": "Tingkat Energi Lebih Tinggi Keadaan Dasar." },
    { "en": "Apa Itu 'Lompatan Kuantum'?", "id": "Transisi Seketika Antar Tingkat Energi." },
    { "en": "Apa Itu 'Spin' (Dalam Kuantum)?", "id": "Momentum Sudut Intrinsik Partikel." },
    { "en": "Apa Itu 'Prinsip Pengecualian Pauli'?", "id": "Dua Fermion Identik Tidak Bisa Sama." },
    { "en": "Apa Itu 'Statistik Fermi-Dirac'?", "id": "Mendeskripsikan Sistem Partikel Fermion." },
    { "en": "Apa Itu 'Statistik Bose-Einstein'?", "id": "Mendeskripsikan Sistem Partikel Boson." },
    { "en": "Apa Itu 'Kondensat Bose-Einstein'?", "id": "Keadaan Materi Pada Suhu Nol Absolut." },
    { "en": "Apa Itu 'Superfluida'?", "id": "Cairan Dengan Viskositas Nol." },
    { "en": "Apa Itu 'Efek Kuantum Hall'?", "id": "Versi Mekanika Kuantum Efek Hall." },
    { "en": "Apa Itu 'Efek Aharonov-Bohm'?", "id": "Partikel Dipengaruhi Potensial Elektromagnetik." },
    { "en": "Apa Itu 'Dekohorensi Kuantum'?", "id": "Hilangnya Sifat Kuantum Akibat Lingkungan." },
    { "en": "Apa Itu 'Komputer Optik'?", "id": "Komputer Yang Menggunakan Foton." },
    { "en": "Apa Itu 'Interferometer'?", "id": "Alat Mengukur Sifat Gelombang." },
    { "en": "Apa Itu 'Difraksi'?", "id": "Penyebaran Gelombang Saat Lewati Celah." },
    { "en": "Apa Itu 'Interferensi'?", "id": "Superposisi Gelombang Yang Menghasilkan Pola." },
    { "en": "Apa Itu 'Holografi'?", "id": "Teknik Merekam Dan Merekonstruksi Gelombang." },
    { "en": "Apa Itu 'Akustik'?", "id": "Ilmu Tentang Suara." },
    { "en": "Apa Itu 'Transduser Ultrasonik'?", "id": "Mengubah Energi Listrik Jadi Ultrasonik." },
    { "en": "Apa Itu 'Sonar' (Sound Navigation and Ranging)?", "id": "Teknik Menggunakan Propagasi Suara." },
    { "en": "Apa Itu 'Kavitasi'?", "id": "Pembentukan Gelembung Uap Dalam Cairan." },
    { "en": "Apa Itu 'Pembatalan Derau Aktif'?", "id": "Mengurangi Derau Dengan Gelombang Antifase." },
    { "en": "Apa Itu 'Filter SAW' (Surface Acoustic Wave)?", "id": "Filter Frekuensi Menggunakan Gelombang Akustik." },
    { "en": "Apa Itu 'Filter BAW' (Bulk Acoustic Wave)?", "id": "Filter Frekuensi Kinerja Lebih Tinggi." },
    { "en": "Apa Itu 'Garis Mikrostrip'?", "id": "Jenis Saluran Transmisi Planar." },
    { "en": "Apa Itu 'Stripline'?", "id": "Saluran Transmisi Diapit Dua Bidang." },
    { "en": "Apa Itu 'Coplanar Waveguide' (CPW)?", "id": "Saluran Transmisi Dengan Konduktor Sebidang." },
    { "en": "Apa Itu 'Substrat'?", "id": "Bahan Dasar Untuk Sirkuit RF." },
    { "en": "Apa Itu 'Resonator Dielektrik'?", "id": "Resonator Frekuensi Tinggi Berbasis Keramik." },
    { "en": "Apa Itu 'Osilator Terkunci Fasa' (PLO)?", "id": "Osilator Dengan Stabilitas Frekuensi Tinggi." },
    { "en": "Apa Itu 'Synthesizer Frekuensi'?", "id": "Membangkitkan Berbagai Frekuensi Dari Referensi." },
    { "en": "Apa Itu 'Synthesizer Langsung' (DDS)?", "id": "Sintesis Sinyal Secara Digital." },
    { "en": "Apa Itu 'Derau Fasa'?", "id": "Fluktuasi Fasa Jangka Pendek Osilator." },
    { "en": "Apa Itu 'Spurious Signals'?", "id": "Sinyal Palsu Tak Diinginkan." },
    { "en": "Apa Itu 'Titik Kompresi 1 dB'?", "id": "Ukuran Linearitas Sebuah Penguat." },
    { "en": "Apa Itu 'Third-Order Intercept Point' (IP3)?", "id": "Ukuran Kinerja Intermodulasi." },
    { "en": "Apa Itu 'Rentang Dinamis' (Dynamic Range)?", "id": "Rasio Antara Sinyal Terkuat Terlemah." },
    { "en": "Apa Itu 'Spurious-Free Dynamic Range' (SFDR)?", "id": "Rentang Dinamis Bebas Sinyal Palsu." },
    { "en": "Apa Itu 'Kuat Medan' (Field Strength)?", "id": "Besaran Medan Elektromagnetik." },
    { "en": "Apa Itu 'Kerapatan Daya'?", "id": "Daya Per Satuan Luas." },
    { "en": "Apa Itu 'Hukum Kuadrat Terbalik'?", "id": "Intensitas Berbanding Terbalik Kuadrat Jarak." },
    { "en": "Apa Itu 'Persamaan Transmisi Friis'?", "id": "Menghitung Daya Diterima Antena." },
    { "en": "Apa Itu 'Propagasi Garis Pandang' (LoS)?", "id": "Jalur Langsung Antara Pemancar Penerima." },
    { "en": "Apa Itu 'Zona Fresnel'?", "id": "Wilayah Elipsoid Sekitar Jalur LoS." },
    { "en": "Apa Itu 'Propagasi Gelombang Tanah'?", "id": "Gelombang Radio Mengikuti Kontur Bumi." },
    { "en": "Apa Itu 'Propagasi Gelombang Langit'?", "id": "Gelombang Radio Dipantulkan Oleh Ionosfer." },
    { "en": "Apa Itu 'Ionosfer'?", "id": "Lapisan Atmosfer Atas Yang Terionisasi." },
    { "en": "Apa Itu 'Redaman Hujan'?", "id": "Pelemahan Sinyal Mikro Akibat Hujan." },
    { "en": "Apa Itu 'Scintillation' (Kerlip)?", "id": "Fluktuasi Cepat Amplitudo/Fasa Sinyal." },
    { "en": "Apa Itu 'Radar' (Radio Detection and Ranging)?", "id": "Sistem Deteksi Menggunakan Gelombang Radio." },
    { "en": "Apa Itu 'Radar Pulsa-Doppler'?", "id": "Radar Yang Mengukur Kecepatan Target." },
    { "en": "Apa Itu 'Phased Array Radar'?", "id": "Radar Dengan Berkas Elektronik Terkendali." },
    { "en": "Apa Itu 'Synthetic Aperture Radar' (SAR)?", "id": "Radar Pencitraan Resolusi Tinggi." },
    { "en": "Apa Itu 'Penampang Lintang Radar' (RCS)?", "id": "Ukuran Kemampuan Target Pantulkan Sinyal." },
    { "en": "Apa Itu 'Chaff'?", "id": "Tindakan Balasan Radar." },
    { "en": "Apa Itu 'Stealth Technology' (Teknologi Siluman)?", "id": "Membuat Objek Sulit Terdeteksi." },
    { "en": "Apa Itu 'Lidar' (Light Detection and Ranging)?", "id": "Radar Menggunakan Sinar Laser." },
    { "en": "Apa Itu 'Navigasi Radio'?", "id": "Menentukan Posisi Menggunakan Sinyal Radio." },
    { "en": "Apa Itu 'VOR' (VHF Omnidirectional Range)?", "id": "Sistem Navigasi Radio Jarak Pendek." },
    { "en": "Apa Itu 'DME' (Distance Measuring Equipment)?", "id": "Mengukur Jarak Antara Pesawat Stasiun." },
    { "en": "Apa Itu 'ILS' (Instrument Landing System)?", "id": "Sistem Pendaratan Pesawat Berbasis Radio." },
    { "en": "Apa Itu 'TCAS' (Traffic Collision Avoidance System)?", "id": "Sistem Pencegah Tabrakan Pesawat." },
    { "en": "Apa Itu 'ADS-B' (Automatic Dependent Surveillance–Broadcast)?", "id": "Teknologi Pengawasan Lalu Lintas Udara." },
    { "en": "Apa Itu 'Penyiaran Radio'?", "id": "Distribusi Konten Audio Ke Audiens." },
    { "en": "Apa Itu 'Penyiaran Televisi'?", "id": "Distribusi Konten Video Ke Audiens." },
    { "en": "Apa Itu 'Radio AM'?", "id": "Penyiaran Menggunakan Modulasi Amplitudo." },
    { "en": "Apa Itu 'Radio FM'?", "id": "Penyiaran Menggunakan Modulasi Frekuensi." },
    { "en": "Apa Itu 'Radio Satelit'?", "id": "Penyiaran Radio Melalui Satelit." },
    { "en": "Apa Itu 'Radio Digital' (HD Radio)?", "id": "Standar Penyiaran Radio Digital." },
    { "en": "Apa Itu 'Televisi Analog'?", "id": "Standar Penyiaran TV Generasi Awal." },
    { "en": "Apa Itu 'Televisi Digital'?", "id": "Standar Penyiaran TV Modern." },
    { "en": "Apa Itu 'NTSC'?", "id": "Standar TV Analog Amerika Utara." },
    { "en": "Apa Itu 'PAL'?", "id": "Standar TV Analog Eropa." },
    { "en": "Apa Itu 'SECAM'?", "id": "Standar TV Analog Prancis Rusia." },
    { "en": "Apa Itu 'ATSC'?", "id": "Standar TV Digital Amerika Utara." },
    { "en": "Apa Itu 'DVB-T'?", "id": "Standar TV Digital Terestrial Eropa." },
    { "en": "Apa Itu 'ISDB-T'?", "id": "Standar TV Digital Jepang Brazil." },
    { "en": "Apa Itu 'Aspek Rasio'?", "id": "Rasio Lebar Terhadap Tinggi Gambar." },
    { "en": "Apa Itu 'Resolusi Layar'?", "id": "Jumlah Piksel Yang Ditampilkan Layar." },
    { "en": "Apa Itu 'Frame Rate'?", "id": "Jumlah Gambar Per Detik." },
    { "en": "Apa Itu 'Interlaced Scan'?", "id": "Metode Pemindaian Gambar Garis Ganjil-Genap." },
    { "en": "Apa Itu 'Progressive Scan'?", "id": "Memindai Semua Garis Gambar Berurutan." },
    { "en": "Apa Itu 'Piksel' (Pixel)?", "id": "Elemen Gambar Terkecil Pada Layar." },
    { "en": "Apa Itu 'Subpixel'?", "id": "Elemen Merah, Hijau, Biru Dalam Piksel." },
    { "en": "Apa Itu 'Color Gamut'?", "id": "Rentang Warna Yang Dapat Ditampilkan." },
    { "en": "Apa Itu 'Ruang Warna RGB'?", "id": "Model Warna Aditif (Merah-Hijau-Biru)." },
    { "en": "Apa Itu 'Ruang Warna CMYK'?", "id": "Model Warna Subtraktif (Cyan-Magenta-Kuning-Hitam)." },
    { "en": "Apa Itu 'Chroma Subsampling'?", "id": "Teknik Kompresi Informasi Warna." },
    { "en": "Apa Itu 'Luminance' (Luma)?", "id": "Komponen Kecerahan Sinyal Video." },
    { "en": "Apa Itu 'Chrominance' (Chroma)?", "id": "Komponen Warna Sinyal Video." },
    { "en": "Apa Itu 'Sinyal Video Komposit'?", "id": "Sinyal Luma Dan Chroma Digabung." },
    { "en": "Apa Itu 'Sinyal S-Video'?", "id": "Sinyal Luma Dan Chroma Terpisah." },
    { "en": "Apa Itu 'Sinyal Video Komponen'?", "id": "Sinyal Luma Dan Warna Terpisah." },
    { "en": "Apa Itu 'HDMI' (High-Definition Multimedia Interface)?", "id": "Standar Antarmuka Audio/Video Digital." },
    { "en": "Apa Itu 'DisplayPort'?", "id": "Antarmuka Video Digital Lainnya." },
    { "en": "Apa Itu 'VGA' (Video Graphics Array)?", "id": "Standar Konektor Tampilan Analog." },
    { "en": "Apa Itu 'DVI' (Digital Visual Interface)?", "id": "Antarmuka Tampilan Video Digital/Analog." },
    { "en": "Apa Itu 'HDCP' (High-bandwidth Digital Content Protection)?", "id": "Skema Perlindungan Salinan Digital." },
    { "en": "Apa Itu 'Codec Video'?", "id": "Mengompres Dan Mendekompres Video Digital." },
    { "en": "Apa Itu 'MPEG' (Moving Picture Experts Group)?", "id": "Standar Kompresi Audio Dan Video." },
    { "en": "Apa Itu 'H.264/AVC'?", "id": "Standar Kompresi Video Populer." },
    { "en": "Apa Itu 'H.265/HEVC'?", "id": "Penerus H.264 Dengan Efisiensi Lebih." },
    { "en": "Apa Itu 'Container Format'?", "id": "Format File Pembungkus Data Multimedia." },
    { "en": "Apa Itu 'Bitrate'?", "id": "Jumlah Bit Per Detik." },
    { "en": "Apa Itu 'Variable Bitrate' (VBR)?", "id": "Bitrate Yang Berubah Sesuai Kompleksitas." },
    { "en": "Apa Itu 'Constant Bitrate' (CBR)?", "id": "Bitrate Yang Tetap Sepanjang Waktu." },
    { "en": "Apa Itu 'Streaming'?", "id": "Mengirim Media Secara Terus Menerus." },
    { "en": "Apa Itu 'IPTV' (Internet Protocol Television)?", "id": "Distribusi Konten Televisi Melalui IP." },
    { "en": "Apa Itu 'VoIP' (Voice over Internet Protocol)?", "id": "Layanan Telepon Melalui Jaringan Internet." },
    { "en": "Apa Itu 'Kodek Audio'?", "id": "Mengompres Dan Mendekompres Audio Digital." },
    { "en": "Apa Itu 'MP3' (MPEG-1 Audio Layer III)?", "id": "Format Kompresi Audio Populer." },
    { "en": "Apa Itu 'AAC' (Advanced Audio Coding)?", "id": "Format Kompresi Audio Lebih Efisien." },
    { "en": "Apa Itu 'FLAC' (Free Lossless Audio Codec)?", "id": "Format Kompresi Audio Tanpa Rugi." },
    { "en": "Apa Itu 'PCM' (Pulse-Code Modulation)?", "id": "Representasi Digital Sinyal Analog." },
    { "en": "Apa Itu 'Kedalaman Bit' (Bit Depth)?", "id": "Jumlah Bit Informasi Per Sampel." },
    { "en": "Apa Itu 'Laju Sampel' (Sample Rate)?", "id": "Jumlah Sampel Sinyal Per Detik." },
    { "en": "Apa Itu 'PSU' (Power Supply Unit)?", "id": "Unit Catu Daya Untuk Komputer." },
    { "en": "Apa Itu 'ATX'?", "id": "Standar Form Factor Untuk PSU/Motherboard." },
    { "en": "Apa Itu 'Motherboard' (Papan Induk)?", "id": "Papan Sirkuit Utama Komputer." },
    { "en": "Apa Itu 'CPU' (Central Processing Unit)?", "id": "Unit Pemrosesan Pusat Komputer." },
    { "en": "Apa Itu 'GPU' (Graphics Processing Unit)?", "id": "Unit Pemrosesan Khusus Untuk Grafis." },
    { "en": "Apa Itu 'Chipset'?", "id": "Sekumpulan Sirkuit Terpadu Di Motherboard." },
    { "en": "Apa Itu 'Northbridge'?", "id": "Chipset Pengontrol Komponen Kecepatan Tinggi." },
    { "en": "Apa Itu 'Southbridge'?", "id": "Chipset Pengontrol Komponen Kecepatan Rendah." },
    { "en": "Apa Itu 'SATA' (Serial AT Attachment)?", "id": "Antarmuka Untuk Menghubungkan Drive Penyimpanan." },
    { "en": "Apa Itu 'PATA' (Parallel AT Attachment)?", "id": "Antarmuka Drive Penyimpanan Generasi Lama." },
    { "en": "Apa Itu 'NVMe' (Non-Volatile Memory Express)?", "id": "Antarmuka Penyimpanan Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'SSD' (Solid-State Drive)?", "id": "Perangkat Penyimpanan Menggunakan Memori Flash." },
    { "en": "Apa Itu 'HDD' (Hard Disk Drive)?", "id": "Perangkat Penyimpanan Menggunakan Piringan Magnetik." },
    { "en": "Apa Itu 'RAID' (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Drive Menjadi Satu." },
    { "en": "Apa Itu 'Hot Swapping'?", "id": "Mengganti Komponen Tanpa Mematikan Sistem." },
    { "en": "Apa Itu 'Form Factor'?", "id": "Standar Ukuran Dan Bentuk Fisik." },
    { "en": "Apa Itu 'Backplane'?", "id": "Papan Sirkuit Dengan Banyak Konektor." },
    { "en": "Apa Itu 'Server Blade'?", "id": "Server Modular Yang Sangat Ramping." },
    { "en": "Apa Itu 'Pusat Data' (Data Center)?", "id": "Fasilitas Penyimpanan Dan Pengolahan Data." },
    { "en": "Apa Itu 'Pendinginan Cair'?", "id": "Sistem Pendinginan Menggunakan Cairan." },
    { "en": "Apa Itu 'PUE' (Power Usage Effectiveness)?", "id": "Ukuran Efisiensi Energi Pusat Data." },
    { "en": "Apa Itu 'Superkomputer'?", "id": "Komputer Dengan Kinerja Sangat Tinggi." },
    { "en": "Apa Itu 'Komputasi Paralel'?", "id": "Menjalankan Banyak Komputasi Secara Bersamaan." },
    { "en": "Apa Itu 'Komputasi Terdistribusi'?", "id": "Komputasi Menggunakan Jaringan Komputer." },
    { "en": "Apa Itu 'Grid Computing'?", "id": "Bentuk Komputasi Terdistribusi Skala Besar." },
    { "en": "Apa Itu 'Quantum Annealing'?", "id": "Metode Optimisasi Menggunakan Efek Kuantum." },
    { "en": "Apa Itu 'Gerbang Kuantum'?", "id": "Operasi Dasar Pada Komputer Kuantum." },
    { "en": "Apa Itu 'Algoritma Shor'?", "id": "Algoritma Kuantum Untuk Faktorisasi." },
    { "en": "Apa Itu 'Algoritma Grover'?", "id": "Algoritma Kuantum Untuk Pencarian." },
    { "en": "Apa Itu 'Kriptografi Kuantum'?", "id": "Kriptografi Menggunakan Prinsip Mekanika Kuantum." },
    { "en": "Apa Itu 'Distribusi Kunci Kuantum' (QKD)?", "id": "Metode Pertukaran Kunci Aman." },
    { "en": "Apa Itu 'Teleportasi Kuantum'?", "id": "Transfer Keadaan Kuantum Jarak Jauh." },
    { "en": "Apa Itu 'Sensor Kuantum'?", "id": "Sensor Yang Memanfaatkan Efek Kuantum." },
    { "en": "Apa Itu 'Jam Atom'?", "id": "Standar Waktu Berdasarkan Resonansi Atom." },
    { "en": "Apa Itu 'Maser'?", "id": "Penguat Gelombang Mikro Berbasis Emisi." },
    { "en": "Apa Itu 'Relativitas Khusus'?", "id": "Teori Einstein Tentang Ruang Waktu." },
    { "en": "Apa Itu 'Relativitas Umum'?", "id": "Teori Einstein Tentang Gravitasi." },
    { "en": "Apa Itu 'Dilatasi Waktu'?", "id": "Perbedaan Waktu Akibat Kecepatan Relatif." },
    { "en": "Apa Itu 'Kontraksi Panjang'?", "id": "Pemendekan Objek Akibat Kecepatan Relatif." },
    { "en": "Apa Itu 'E=mc²'?", "id": "Persamaan Kesetaraan Massa-Energi." },
    { "en": "Apa Itu 'Lensa Gravitasi'?", "id": "Pembelokan Cahaya Oleh Objek Masif." },
    { "en": "Apa Itu 'Gelombang Gravitasi'?", "id": "Riak Dalam Ruang-Waktu." },
    { "en": "Apa Itu 'Lubang Hitam'?", "id": "Wilayah Ruang-Waktu Dengan Gravitasi Kuat." },
    { "en": "Apa Itu 'Horison Peristiwa'?", "id": "Batas Lubang Hitam." },
    { "en": "Apa Itu 'Singularitas'?", "id": "Titik Kepadatan Tak Terhingga." },
    { "en": "Apa Itu 'Radiasi Hawking'?", "id": "Radiasi Termal Hipotetis Lubang Hitam." },
    { "en": "Apa Itu 'Lubang Cacing' (Wormhole)?", "id": "Jalan Pintas Hipotetis Ruang-Waktu." },
    { "en": "Apa Itu 'Materi Gelap'?", "id": "Materi Hipotetis Tak Berinteraksi." },
    { "en": "Apa Itu 'Energi Gelap'?", "id": "Energi Hipotetis Penyebab Ekspansi Semesta." },
    { "en": "Apa Itu 'Dentuman Besar' (Big Bang)?", "id": "Teori Awal Mula Alam Semesta." },
    { "en": "Apa Itu 'Radiasi Latar Belakang Kosmik'?", "id": "Sisa Cahaya Dari Alam Semesta Awal." },
    { "en": "Apa Itu 'Pergeseran Merah' (Redshift)?", "id": "Pergeseran Spektrum Akibat Jarak Kosmik." },
    { "en": "Apa Itu 'Hukum Hubble'?", "id": "Hubungan Kecepatan Galaksi Dengan Jarak." },
    { "en": "Apa Itu 'Efek Unruh'?", "id": "Prediksi Pengamat Dipercepat Deteksi Radiasi." },
    { "en": "Apa Itu 'Teori String'?", "id": "Model Fisika Dengan Partikel Sebagai String." },
    { "en": "Apa Itu 'Teori-M'?", "id": "Teori Yang Menyatukan Teori String." },
    { "en": "Apa Itu 'Dimensi Ekstra'?", "id": "Dimensi Hipotetis Selain Empat Dimensi." },
    { "en": "Apa Itu 'Prinsip Holografik'?", "id": "Deskripsi Volume Ruang Pada Batasnya." },
    { "en": "Apa Itu 'Multiverse' (Multisemesta)?", "id": "Kumpulan Hipotetis Banyak Alam Semesta." },
    { "en": "Apa Itu 'Sistem Tertanam' (Embedded System)?", "id": "Sistem Komputer Dengan Fungsi Spesifik." },
    { "en": "Apa Itu 'Real-Time' (Waktu Nyata)?", "id": "Sistem Yang Merespon Dalam Batas Waktu." },
    { "en": "Apa Itu 'Hard Real-Time'?", "id": "Batas Waktu Yang Sama Sekali Tidak Boleh Terlewat." },
    { "en": "Apa Itu 'Soft Real-Time'?", "id": "Batas Waktu Yang Boleh Terlewat Sesekali." },
    { "en": "Apa Itu 'Cross-Compiler'?", "id": "Compiler Yang Berjalan Di Platform Berbeda." },
    { "en": "Apa Itu 'In-Circuit Emulator' (ICE)?", "id": "Alat Debugging Untuk Sistem Tertanam." },
    { "en": "Apa Itu 'Breakpoint'?", "id": "Titik Henti Sementara Eksekusi Program." },
    { "en": "Apa Itu 'Single Stepping'?", "id": "Menjalankan Program Satu Instruksi Sekaligus." },
    { "en": "Apa Itu 'Profil Pemakaian Daya' (Power Profiling)?", "id": "Menganalisis Konsumsi Daya Perangkat." },
    { "en": "Apa Itu 'Mode Tidur' (Sleep Mode)?", "id": "Mode Operasi Hemat Daya." },
    { "en": "Apa Itu 'Siklus Bangun-Tidur'?", "id": "Pola Hemat Daya Untuk Perangkat IoT." },
    { "en": "Apa Itu 'Tegangan Inti' (Core Voltage)?", "id": "Tegangan Yang Disuplai Ke Inti Prosesor." },
    { "en": "Apa Itu 'Tegangan I/O'?", "id": "Tegangan Untuk Komunikasi Input/Output." },
    { "en": "Apa Itu 'Level Shifter'?", "id": "Mengubah Level Tegangan Sinyal Logika." },
    { "en": "Apa Itu 'Open Drain'?", "id": "Jenis Keluaran Yang Membutuhkan Pull-up." },
    { "en": "Apa Itu 'Antarmuka JTAG'?", "id": "Antarmuka Untuk Debugging Dan Pemrograman." },
    { "en": "Apa Itu 'Antarmuka SWD' (Serial Wire Debug)?", "id": "Antarmuka Debug Dua Kawat." },
    { "en": "Apa Itu 'ISP' (In-System Programming)?", "id": "Memprogram Perangkat Saat Terpasang." },
    { "en": "Apa Itu 'Boot ROM'?", "id": "ROM Kecil Berisi Kode Bootloader Awal." },
    { "en": "Apa Itu 'Memory-Mapped I/O'?", "id": "Mengakses Periferal Seperti Lokasi Memori." },
    { "en": "Apa Itu 'Port-Mapped I/O'?", "id": "Menggunakan Instruksi Khusus Akses Periferal." },
    { "en": "Apa Itu 'Vektor Reset'?", "id": "Alamat Awal Eksekusi Setelah Reset." },
    { "en": "Apa Itu 'Power-On Reset' (POR)?", "id": "Sirkuit Pembangkit Sinyal Reset Otomatis." },
    { "en": "Apa Itu 'Sistem-Kaya-Fitur' (Feature-Rich System)?", "id": "Sistem Kompleks Menjalankan OS Lengkap." },
    { "en": "Apa Itu 'Sistem-Terbatas-Sumberdaya'?", "id": "Sistem Dengan Memori/Daya Terbatas." },
    { "en": "Apa Itu 'Perancangan Berbasis Model' (MBD)?", "id": "Menggunakan Model Grafis Untuk Desain." },
    { "en": "Apa Itu 'Simulink'?", "id": "Lingkungan Pemodelan Grafis Dari MathWorks." },
    { "en": "Apa Itu 'HIL' (Hardware-in-the-Loop)?", "id": "Teknik Pengujian Dengan Perangkat Keras." },
    { "en": "Apa Itu 'SIL' (Software-in-the-Loop)?", "id": "Pengujian Kode Tertanam Secara Virtual." },
    { "en": "Apa Itu 'Verifikasi dan Validasi' (V&V)?", "id": "Proses Memastikan Kualitas Produk." },
    { "en": "Apa Itu 'Analisis Statis Kode'?", "id": "Menganalisis Kode Sumber Tanpa Menjalankannya." },
    { "en": "Apa Itu 'Unit Testing'?", "id": "Pengujian Komponen Perangkat Lunak Individual." },
    { "en": "Apa Itu 'Integration Testing'?", "id": "Pengujian Interaksi Antar Komponen." },
    { "en": "Apa Itu 'System Testing'?", "id": "Pengujian Sistem Lengkap Secara Keseluruhan." },
    { "en": "Apa Itu 'Regression Testing'?", "id": "Memastikan Perubahan Tidak Merusak Fungsi." },
    { "en": "Apa Itu 'Code Coverage'?", "id": "Ukuran Seberapa Banyak Kode Dijalankan." },
    { "en": "Apa Itu 'Standar Pengkodean' (Coding Standard)?", "id": "Aturan Dan Pedoman Penulisan Kode." },
    { "en": "Apa Itu 'MISRA C'?", "id": "Standar Pengkodean C Untuk Sistem Kritis." },
    { "en": "Apa Itu 'Siklus Hidup Pengembangan Perangkat Lunak' (SDLC)?", "id": "Proses Untuk Membangun Perangkat Lunak." },
    { "en": "Apa Itu 'Model Air Terjun' (Waterfall)?", "id": "Model SDLC Linier Sekuensial." },
    { "en": "Apa Itu 'Model V'?", "id": "Perpanjangan Dari Model Air Terjun." },
    { "en": "Apa Itu 'Metodologi Agile'?", "id": "Pendekatan Pengembangan Perangkat Lunak Iteratif." },
    { "en": "Apa Itu 'Scrum'?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
    { "en": "Apa Itu 'Kanban'?", "id": "Metode Manajemen Alur Kerja Visual." },
    { "en": "Apa Itu 'Kontrol Versi' (Version Control)?", "id": "Sistem Perekam Perubahan Kode." },
    { "en": "Apa Itu 'Git'?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
    { "en": "Apa Itu 'Repository'?", "id": "Penyimpanan Pusat Untuk Kode Proyek." },
    { "en": "Apa Itu 'Branch' (Cabang)?", "id": "Garis Pengembangan Independen Dalam Git." },
    { "en": "Apa Itu 'Merge' (Menggabungkan)?", "id": "Menggabungkan Perubahan Dari Cabang Berbeda." },
    { "en": "Apa Itu 'Pull Request'?", "id": "Permintaan Untuk Menggabungkan Kode." },
    { "en": "Apa Itu 'Continuous Integration' (CI)?", "id": "Praktik Mengintegrasikan Kode Secara Berkala." },
    { "en": "Apa Itu 'Continuous Deployment' (CD)?", "id": "Praktik Merilis Perangkat Lunak Otomatis." },
    { "en": "Apa Itu 'DevOps'?", "id": "Kombinasi Pengembangan Dan Operasi." },
    { "en": "Apa Itu 'Penghitung Kinerja' (Performance Counter)?", "id": "Hardware Untuk Memantau Kinerja CPU." },
    { "en": "Apa Itu 'Analisis Waktu Eksekusi Terburuk' (WCET)?", "id": "Menentukan Waktu Eksekusi Maksimum Tugas." },
    { "en": "Apa Itu 'Penjadwalan' (Scheduling)?", "id": "Menentukan Tugas Mana Dijalankan Kapan." },
    { "en": "Apa Itu 'Penjadwal Preemptive'?", "id": "Dapat Menginterupsi Tugas Yang Berjalan." },
    { "en": "Apa Itu 'Penjadwal Non-Preemptive'?", "id": "Tugas Berjalan Sampai Selesai." },
    { "en": "Apa Itu 'Prioritas Tugas'?", "id": "Tingkat Kepentingan Relatif Sebuah Tugas." },
    { "en": "Apa Itu 'Inversi Prioritas'?", "id": "Tugas Prioritas Rendah Menghalangi Tinggi." },
    { "en": "Apa Itu 'Mutex' (Mutual Exclusion)?", "id": "Mekanisme Pencegah Akses Simultan." },
    { "en": "Apa Itu 'Semaphore'?", "id": "Variabel Untuk Mengontrol Akses Sumberdaya." },
    { "en": "Apa Itu 'Deadlock'?", "id": "Dua Tugas Saling Menunggu Selamanya." },
    { "en": "Apa Itu 'Starvation' (Kelaparan)?", "id": "Tugas Tidak Pernah Mendapat Giliran." },
    { "en": "Apa Itu 'Komunikasi Antar-Proses' (IPC)?", "id": "Mekanisme Pertukaran Data Antar Tugas." },
    { "en": "Apa Itu 'Message Queue' (Antrian Pesan)?", "id": "IPC Menggunakan Antrian Pesan." },
    { "en": "Apa Itu 'Shared Memory' (Memori Bersama)?", "id": "IPC Menggunakan Area Memori Sama." },
    { "en": "Apa Itu 'Socket'?", "id": "Titik Akhir Komunikasi Jaringan." },
    { "en": "Apa Itu 'Little's Law'?", "id": "Hubungan Antara Throughput, Latency, Konkurensi." },
    { "en": "Apa Itu 'Amdahl's Law'?", "id": "Batas Peningkatan Kinerja Teoritis." },
    { "en": "Apa Itu 'Gustafson's Law'?", "id": "Pandangan Alternatif Terhadap Hukum Amdahl." },
    { "en": "Apa Itu 'Cache Coherence'?", "id": "Menjaga Konsistensi Data Antar Cache." },
    { "en": "Apa Itu 'False Sharing'?", "id": "Penurunan Kinerja Akibat Koherensi Cache." },
    { "en": "Apa Itu 'Lock-Free Programming'?", "id": "Pemrograman Tanpa Menggunakan Mutex." },
    { "en": "Apa Itu 'Operasi Atomik'?", "id": "Operasi Yang Tidak Dapat Diinterupsi." },
    { "en": "Apa Itu 'Memory Barrier'?", "id": "Memastikan Urutan Operasi Memori." },
    { "en": "Apa Itu 'Data Race'?", "id": "Akses Simultan Tak Terkoordinasi." },
    { "en": "Apa Itu 'Gantt Chart'?", "id": "Bagan Batang Ilustrasi Jadwal Proyek." },
    { "en": "Apa Itu 'PERT Chart'?", "id": "Alat Manajemen Proyek Untuk Penjadwalan." },
    { "en": "Apa Itu 'Critical Path Method' (CPM)?", "id": "Algoritma Penjadwalan Proyek." },
    { "en": "Apa Itu 'Work Breakdown Structure' (WBS)?", "id": "Pemecahan Proyek Menjadi Tugas Kecil." },
    { "en": "Apa Itu 'Milestone' (Tonggak Sejarah)?", "id": "Titik Spesifik Sepanjang Linimasa Proyek." },
    { "en": "Apa Itu 'Manajemen Risiko'?", "id": "Identifikasi Dan Mitigasi Potensi Masalah." },
    { "en": "Apa Itu 'Manajemen Kualitas'?", "id": "Memastikan Produk Memenuhi Standar." },
    { "en": "Apa Itu 'Kekayaan Intelektual' (IP)?", "id": "Ciptaan Pikiran (Paten, Hak Cipta)." },
    { "en": "Apa Itu 'Paten'?", "id": "Hak Eksklusif Untuk Sebuah Penemuan." },
    { "en": "Apa Itu 'Hak Cipta'?", "id": "Hak Hukum Atas Karya Orisinal." },
    { "en": "Apa Itu 'Merek Dagang'?", "id": "Simbol Pembeda Barang Atau Jasa." },
    { "en": "Apa Itu 'Rahasia Dagang'?", "id": "Informasi Rahasia Yang Memberi Keunggulan." },
    { "en": "Apa Itu 'Non-Disclosure Agreement' (NDA)?", "id": "Perjanjian Kerahasiaan." },
    { "en": "Apa Itu 'Open Source' (Sumber Terbuka)?", "id": "Perangkat Lunak Dengan Kode Sumber Terbuka." },
    { "en": "Apa Itu 'Lisensi Permisif'?", "id": "Lisensi Open Source Dengan Sedikit Batasan." },
    { "en": "Apa Itu 'Lisensi Copyleft'?", "id": "Lisensi Yang Mewajibkan Karya Turunan." },
    { "en": "Apa Itu 'Etika Rekayasa'?", "id": "Prinsip Moral Untuk Praktik Rekayasa." },
    { "en": "Apa Itu 'Konflik Kepentingan'?", "id": "Situasi Mengganggu Objektivitas Profesional." },
    { "en": "Apa Itu 'Whistleblowing'?", "id": "Mengungkap Pelanggaran Dalam Organisasi." }




        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
