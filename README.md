<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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

  { "en": "Daku?", "id": "Aku." },
  { "en": "Dakwa?", "id": "Tuduh." },
  { "en": "Dal?", "id": "Huruf Arab." },
  { "en": "Dala?", "id": "Kosong (arkais)." },
  { "en": "Dalal?", "id": "Perantara (Arab)." },
  { "en": "Dalam?", "id": "Interior." },
  { "en": "Dalang?", "id": "Pemain wayang." },
  { "en": "Daldaru?", "id": "Pohon." },
  { "en": "Dalem?", "id": "Rumah bangsawan (Jawa)." },
  { "en": "Dalfin?", "id": "Lumba-lumba." },
  { "en": "Dalih?", "id": "Alasan." },
  { "en": "Dalil?", "id": "Bukti." },
  { "en": "Dalu?", "id": "Malam (Jawa)." },
  { "en": "Dalung?", "id": "Nampan besar." },
  { "en": "Dam?", "id": "Bendungan." },
  { "en": "Damai?", "id": "Aman." },
  { "en": "Damak?", "id": "Anak panah sumpitan." },
  { "en": "Damal?", "id": "Obor (arkais)." },
  { "en": "Daman?", "id": "Jaminan (Arab)." },
  { "en": "Damar?", "id": "Resin pohon." },
  { "en": "Damas?", "id": "Kain." },
  { "en": "Damat?", "id": "Riuh." },
  { "en": "Damba?", "id": "Rindu." },
  { "en": "Dame?", "id": "Damai (arkais)." },
  { "en": "Dami?", "id": "Jerami (arkais)." },
  { "en": "Damik?", "id": "Tampar (arkais)." },
  { "en": "Damotin?", "id": "Dinamit (arkais)." },
  { "en": "Dampak?", "id": "Akibat." },
  { "en": "Dampar?", "id": "Terdampar." },
  { "en": "Dampel?", "id": "Tumbuk (arkais)." },
  { "en": "Damping?", "id": "Sanding." },
  { "en": "Dampit?", "id": "Kembar (Jawa)." },
  { "en": "Damprat?", "id": "Hardik." },
  { "en": "Dan?", "id": "Serta." },
  { "en": "Dana?", "id": "Uang." },
  { "en": "Danau?", "id": "Telaga." },
  { "en": "Danawa?", "id": "Raksasa." },
  { "en": "Danda?", "id": "Hukuman (Sanskerta)." },
  { "en": "Dandan?", "id": "Hias." },
  { "en": "Dandanggula?", "id": "Tembang Jawa." },
  { "en": "Dandi?", "id": "Tongkat (India)." },
  { "en": "Dang?", "id": "Panggilan hormat (wanita)." },
  { "en": "Dangai?", "id": "Kue." },
  { "en": "Dangak?", "id": "Tengadah." },
  { "en": "Dangar?", "id": "Keras kepala (arkais)." },
  { "en": "Dangau?", "id": "Gubuk." },
  { "en": "Dangdut?", "id": "Musik." },
  { "en": "Dange?", "id": "Makanan." },
  { "en": "Danghyang?", "id": "Roh suci (Bali)." },
  { "en": "Dangir?", "id": "Siangi rumput." },
  { "en": "Dangka?", "id": "Dangkal (arkais)." },
  { "en": "Dangkal?", "id": "Cetek." },
  { "en": "Dangkap?", "id": "Dekap." },
  { "en": "Dangkung?", "id": "Jongkok (arkais)." },
  { "en": "Dansa?", "id": "Tari." },
  { "en": "Danta?", "id": "Gading (Sanskerta)." },
  { "en": "Danuh?", "id": "Danau (arkais)." },
  { "en": "Dap?", "id": "Perisai." },
  { "en": "Dapar?", "id": "Lari cepat." },
  { "en": "Dapat?", "id": "Bisa." },
  { "en": "Dapra?", "id": "Panggung (arkais)." },
  { "en": "Dapur?", "id": "Tempat masak." },
  { "en": "Dar?", "id": "Negeri (Arab)." },
  { "en": "Dara?", "id": "Gadis." },
  { "en": "Darab?", "id": "Kali (hitung)." },
  { "en": "Darah?", "id": "Zat cair tubuh." },
  { "en": "Darajat?", "id": "Derajat." },
  { "en": "Daras?", "id": "Baca cepat." },
  { "en": "Darat?", "id": "Tanah." },
  { "en": "Dari?", "id": "Asal." },
  { "en": "Daripada?", "id": "Dibanding." },
  { "en": "Darji?", "id": "Penjahit (India)." },
  { "en": "Darma?", "id": "Kewajiban." },
  { "en": "Darmabakti?", "id": "Pengabdian." },
  { "en": "Darmakelana?", "id": "Pengembara." },
  { "en": "Darmasiswa?", "id": "Beasiswa." },
  { "en": "Darmawisata?", "id": "Piknik." },
  { "en": "Daru-daru?", "id": "Pohon." },
  { "en": "Darulaitam?", "id": "Panti asuhan." },
  { "en": "Darum?", "id": "Jarum (arkais)." },
  { "en": "Darunu?", "id": "Merpati (arkais)." },
  { "en": "Darurat?", "id": "Gawat." },
  { "en": "Darusalam?", "id": "Negeri damai." },
  { "en": "Darwis?", "id": "Sufi." },
  { "en": "Das?", "id": "Tembakan." },
  { "en": "Dasa?", "id": "Sepuluh (Sanskerta)." },
  { "en": "Dasar?", "id": "Asas." },
  { "en": "Dasarian?", "id": "Periode sepuluh tahun." },
  { "en": "Dasasila?", "id": "Sepuluh prinsip." },
  { "en": "Dasawarsa?", "id": "Dekade." },
  { "en": "Dasi?", "id": "Aksesori leher." },
  { "en": "Dastar?", "id": "Ikat kepala." },
  { "en": "Data?", "id": "Fakta." },
  { "en": "Datang?", "id": "Tiba." },
  { "en": "Datar?", "id": "Rata." },
  { "en": "Datif?", "id": "Kasus tata bahasa." },
  { "en": "Dati?", "id": "Tanah (arkais)." },
  { "en": "Datuk?", "id": "Kakek." },
  { "en": "Datum?", "id": "Data tunggal." },
  { "en": "Datu?", "id": "Kepala suku." },
  { "en": "Daulat?", "id": "Kekuasaan." },
  { "en": "Daun?", "id": "Bagian tumbuhan." },
  { "en": "Daur?", "id": "Siklus." },
  { "en": "Daut?", "id": "Pancing (arkais)." },
  { "en": "Dawai?", "id": "Senar." },
  { "en": "Dawas?", "id": "Bingung (arkais)." },
  { "en": "Dawet?", "id": "Minuman." },
  { "en": "Daya?", "id": "Kekuatan." },
  { "en": "Dayah?", "id": "Pesantren (Aceh)." },
  { "en": "Dayang?", "id": "Pelayan wanita istana." },
  { "en": "Dayu?", "id": "Gema (arkais)." },
  { "en": "Dayuh?", "id": "Tamu (arkais)." },
  { "en": "Dayuk?", "id": "Meratap (arkais)." },
  { "en": "Dayung?", "id": "Pendayung." },
  { "en": "Dayus?", "id": "Pria tak cemburu." },
  { "en": "Deal?", "id": "Kesepakatan (Inggris)." },
  { "en": "Debar?", "id": "Denyut." },
  { "en": "Debarkasi?", "id": "Pendaratan." },
  { "en": "Debat?", "id": "Diskusi." },
  { "en": "Debet?", "id": "Debit." },
  { "en": "Debil?", "id": "Lemah mental." },
  { "en": "Debit?", "id": "Volume air mengalir." },
  { "en": "Debitor?", "id": "Peminjam." },
  { "en": "Debitur?", "id": "Debitor." },
  { "en": "Debu?", "id": "Serbuk halus." },
  { "en": "Debug?", "id": "Perbaiki galat (komputer)." },
  { "en": "Debum?", "id": "Bunyi jatuh." },
  { "en": "Debung?", "id": "Bambu muda (arkais)." },
  { "en": "Debup?", "id": "Bunyi pukulan." },
  { "en": "Debur?", "id": "Bunyi ombak." },
  { "en": "Debut?", "id": "Penampilan pertama." },
  { "en": "Decak?", "id": "Bunyi lidah." },
  { "en": "Decap?", "id": "Bunyi makan." },
  { "en": "Decit?", "id": "Bunyi tikus." },
  { "en": "Decup?", "id": "Bunyi isapan." },
  { "en": "Decur?", "id": "Pancur." },
  { "en": "Dedah?", "id": "Terbuka." },
  { "en": "Dedai?", "id": "Burung." },
  { "en": "Dedak?", "id": "Kulit padi." },
  { "en": "Dedal?", "id": "Pelindung jari." },
  { "en": "Dedap?", "id": "Pohon." },
  { "en": "Dedar?", "id": "Demam." },
  { "en": "Dedes?", "id": "Musang." },
  { "en": "Dedikasi?", "id": "Pengabdian." },
  { "en": "Deduksi?", "id": "Kesimpulan." },
  { "en": "Deduktif?", "id": "Dari umum ke khusus." },
  { "en": "Deeskalasi?", "id": "Pengurangan." },
  { "en": "Defaitisme?", "id": "Sikap menyerah." },
  { "en": "Defekasi?", "id": "Buang air besar." },
  { "en": "Defensi?", "id": "Pertahanan." },
  { "en": "Defensif?", "id": "Bertahan." },
  { "en": "Deferensiasi?", "id": "Pembedaan." },
  { "en": "Defile?", "id": "Parade." },
  { "en": "Definisi?", "id": "Batasan." },
  { "en": "Definit?", "id": "Pasti." },
  { "en": "Definitif?", "id": "Sudah pasti." },
  { "en": "Defisiensi?", "id": "Kekurangan." },
  { "en": "Defisit?", "id": "Kekurangan anggaran." },
  { "en": "Deflagrasi?", "id": "Pembakaran cepat." },
  { "en": "Deflasi?", "id": "Penurunan harga." },
  { "en": "Defleksi?", "id": "Penyimpangan." },
  { "en": "Deflorasi?", "id": "Perusakan selaput dara." },
  { "en": "Defoliasi?", "id": "Pengguguran daun." },
  { "en": "Deformasi?", "id": "Perubahan bentuk." },
  { "en": "Degam?", "id": "Bunyi." },
  { "en": "Degan?", "id": "Kelapa muda." },
  { "en": "Degap?", "id": "Tegap." },
  { "en": "Degar?", "id": "Keras." },
  { "en": "Degenerasi?", "id": "Kemunduran." },
  { "en": "Degil?", "id": "Keras kepala." },
  { "en": "Degradasi?", "id": "Penurunan mutu." },
  { "en": "Degub?", "id": "Debar." },
  { "en": "Deguk?", "id": "Bunyi telan." },
  { "en": "Degum?", "id": "Bunyi berat." },
  { "en": "Deh?", "id": "Partikel penegas." },
  { "en": "Deham?", "id": "Batuk kecil." },
  { "en": "Dehidrat?", "id": "Kering." },
  { "en": "Dehidrasi?", "id": "Kekurangan cairan." },
  { "en": "Deifikasi?", "id": "Penuhanan." },
  { "en": "Dek?", "id": "Adik (panggilan)." },
  { "en": "Dekad?", "id": "Dekade." },
  { "en": "Dekade?", "id": "Dasawarsa." },
  { "en": "Dekaden?", "id": "Merosot moral." },
  { "en": "Dekadensi?", "id": "Kemerosotan moral." },
  { "en": "Dekagram?", "id": "Satuan berat." },
  { "en": "Dekaliter?", "id": "Satuan volume." },
  { "en": "Dekam?", "id": "Mendekam." },
  { "en": "Dekameter?", "id": "Satuan panjang." },
  { "en": "Dekan?", "id": "Pemimpin fakultas." },
  { "en": "Dekap?", "id": "Peluk." },
  { "en": "Dekar?", "id": "Serangga." },
  { "en": "Dekat?", "id": "Hampir." },
  { "en": "Dekih?", "id": "Kotoran." },
  { "en": "Dekil?", "id": "Sangat kotor." },
  { "en": "Deklamasi?", "id": "Pembacaan sajak." },
  { "en": "Deklarasi?", "id": "Pernyataan." },
  { "en": "Deklinasi?", "id": "Penurunan." },
  { "en": "Dekode?", "id": "Urai sandi." },
  { "en": "Dekok?", "id": "Lekuk." },
  { "en": "Dekomposer?", "id": "Pengurai." },
  { "en": "Dekomposisi?", "id": "Penguraian." },
  { "en": "Dekongestan?", "id": "Obat pilek." },
  { "en": "Dekor?", "id": "Hiasan." },
  { "en": "Dekorasi?", "id": "Hiasan." },
  { "en": "Dekorum?", "id": "Tata krama." },
  { "en": "Dekret?", "id": "Keputusan." },
  { "en": "Dekriminalisasi?", "id": "Penghapusan pidana." },
  { "en": "Deksi?", "id": "Botol." },
  { "en": "Dekstrin?", "id": "Zat tepung." },
  { "en": "Dekstrosa?", "id": "Gula." },
  { "en": "Dekul?", "id": "Sombong (arkais)." },
  { "en": "Dekumben?", "id": "Berbaring." },
  { "en": "Dekunci?", "id": "Kunci (arkais)." },
  { "en": "Dekung?", "id": "Bunyi." },
  { "en": "Dekur?", "id": "Bunyi merpati." },
  { "en": "Dekus?", "id": "Dengkus." },
  { "en": "Delabialisas?", "id": "Perubahan bunyi." },
  { "en": "Delah?", "id": "Sombong (arkais)." },
  { "en": "Delamak?", "id": "Alas makan." },
  { "en": "Delan?", "id": "Nama ikan." },
  { "en": "Delap?", "id": "Kilau (arkais)." },
  { "en": "Delapan?", "id": "Angka." },
  { "en": "Delas?", "id": "Tulis (arkais)." },
  { "en": "Delegasi?", "id": "Utusan." },
  { "en": "Delepak?", "id": "Bunyi." },
  { "en": "Delik?", "id": "Pelanggaran hukum." },
  { "en": "Delikan?", "id": "Jelai." },
  { "en": "Delima?", "id": "Buah." },
  { "en": "Delinkuensi?", "id": "Kenakalan." },
  { "en": "Delirium?", "id": "Mengigau." },
  { "en": "Delman?", "id": "Kereta kuda." },
  { "en": "Delong?", "id": "Belok (arkais)." },
  { "en": "Delongop?", "id": "Ternganga." },
  { "en": "Delta?", "id": "Endapan sungai." },
  { "en": "Delu?", "id": "Tempayan (arkais)." },
  { "en": "Deluang?", "id": "Kertas kulit kayu." },
  { "en": "Delujur?", "id": "Jahit jarang." },
  { "en": "Delusi?", "id": "Khayalan." },
  { "en": "Demabrasi?", "id": "Pengikisan kulit." },
  { "en": "Demagog?", "id": "Penghasut rakyat." },
  { "en": "Demagogi?", "id": "Hasutan." },
  { "en": "Demah?", "id": "Kompres." },
  { "en": "Demam?", "id": "Sakit panas." },
  { "en": "Demang?", "id": "Kepala distrik." },
  { "en": "Demarkasi?", "id": "Garis batas." },
  { "en": "Dembam?", "id": "Bunyi." },
  { "en": "Dembun?", "id": "Timbun (arkais)." },
  { "en": "Demi?", "id": "Untuk." },
  { "en": "Demik?", "id": "Sempit (arkais)." },
  { "en": "Demikian?", "id": "Begitu." },
  { "en": "Demiliterisas?", "id": "Pengurangan militer." },
  { "en": "Demineralisas?", "id": "Penghilangan mineral." },
  { "en": "Demisioner?", "id": "Habis jabatan." },
  { "en": "Demo?", "id": "Unjuk rasa." },
  { "en": "Demobilisan?", "id": "Prajurit nonaktif." },
  { "en": "Demobilisasi?", "id": "Pengurangan tentara." },
  { "en": "Demograf?", "id": "Ahli kependudukan." },
  { "en": "Demografi?", "id": "Ilmu kependudukan." },
  { "en": "Demokrasi?", "id": "Kedaulatan rakyat." },
  { "en": "Demon?", "id": "Setan." },
  { "en": "Demonologi?", "id": "Ilmu setan." },
  { "en": "Demonstran?", "id": "Pengunjuk rasa." },
  { "en": "Demonstrasi?", "id": "Peragaan." },
  { "en": "Demoralisasi?", "id": "Penurunan moral." },
  { "en": "Demos?", "id": "Rakyat (Yunani)." },
  { "en": "Dempak?", "id": "Rendah pipih." },
  { "en": "Dempam?", "id": "Benturan." },
  { "en": "Dempet?", "id": "Rapat." },
  { "en": "Dempuk?", "id": "Bertemu (arkais)." },
  { "en": "Demplon?", "id": "Montok." },
  { "en": "Demung?", "id": "Alat gamelan." },
  { "en": "Dena?", "id": "Orang (Jawa)." },
  { "en": "Denah?", "id": "Peta." },
  { "en": "Denai?", "id": "Jejak." },
  { "en": "Denak?", "id": "Umpan." },
  { "en": "Denasionalisa?", "id": "Pengembalian ke swasta." },
  { "en": "Denda?", "id": "Hukuman uang." },
  { "en": "Dendam?", "id": "Benci mendalam." },
  { "en": "Dendan?", "id": "Berdandan (arkais)." },
  { "en": "Dendang?", "id": "Nyanyian." },
  { "en": "Dendeng?", "id": "Daging kering." },
  { "en": "Dendi?", "id": "Pesolek." },
  { "en": "Denervasi?", "id": "Penghilangan saraf." },
  { "en": "Dengan?", "id": "Bersama." },
  { "en": "Dengap?", "id": "Lemah (arkais)." },
  { "en": "Dengar?", "id": "Rungu." },
  { "en": "Dengih?", "id": "Bau busuk." },
  { "en": "Denging?", "id": "Dengung." },
  { "en": "Dengkak?", "id": "Bunyi katak." },
  { "en": "Dengkel?", "id": "Kering (dahan)." },
  { "en": "Dengki?", "id": "Iri." },
  { "en": "Dengkol?", "id": "Bengkok (kaki)." },
  { "en": "Dengkul?", "id": "Lutut." },
  { "en": "Dengkung?", "id": "Bunyi gong." },
  { "en": "Dengkur?", "id": "Ngorok." },
  { "en": "Dengkus?", "id": "Bunyi napas." },
  { "en": "Dengu?", "id": "Bodoh (arkais)." },
  { "en": "Dengue?", "id": "Penyakit demam." },
  { "en": "Denguk?", "id": "Menunduk." },
  { "en": "Dengung?", "id": "Bunyi bergema." },
  { "en": "Denier?", "id": "Ukuran serat." },
  { "en": "Denizet?", "id": "Mendikte (arkais)." },
  { "en": "Denok?", "id": "Panggilan gadis (Jawa)." },
  { "en": "Denominasi?", "id": "Golongan." },
  { "en": "Denotasi?", "id": "Makna harfiah." },
  { "en": "Dentam?", "id": "Bunyi keras." },
  { "en": "Dentang?", "id": "Bunyi lonceng." },
  { "en": "Dentat?", "id": "Bergigi." },
  { "en": "Denting?", "id": "Bunyi logam." },
  { "en": "Dentum?", "id": "Bunyi meriam." },
  { "en": "Dentur?", "id": "Bunyi." },
  { "en": "Denudasi?", "id": "Penggundulan." },
  { "en": "Denuklirisasi?", "id": "Bebas nuklir." },
  { "en": "Denyar?", "id": "Kilat." },
  { "en": "Denyut?", "id": "Gerak." },
  { "en": "Deodoran?", "id": "Penghilang bau." },
  { "en": "Depa?", "id": "Ukuran panjang." },
  { "en": "Depak?", "id": "Tendang." },
  { "en": "Depan?", "id": "Muka." },
  { "en": "Departemen?", "id": "Bagian." },
  { "en": "Dependen?", "id": "Bergantung." },
  { "en": "Dependensi?", "id": "Ketergantungan." },
  { "en": "Depersonalisasi?", "id": "Kehilangan identitas." },
  { "en": "Depigmentasi?", "id": "Hilang pigmen." },
  { "en": "Depilasi?", "id": "Penghilangan rambut." },
  { "en": "Depleci?", "id": "Pengurangan." },
  { "en": "Deplesi?", "id": "Kekurangan." },
  { "en": "Depolarisasi?", "id": "Pengurangan polarisasi." },
  { "en": "Deponir?", "id": "Simpan." },
  { "en": "Depopulasi?", "id": "Pengurangan penduduk." },
  { "en": "Deportasi?", "id": "Pengusiran." },
  { "en": "Deposan?", "id": "Penyimpan uang." },
  { "en": "Deposit?", "id": "Simpanan." },
  { "en": "Deposito?", "id": "Simpanan berjangka." },
  { "en": "Depot?", "id": "Gudang." },
  { "en": "Depresi?", "id": "Kemurungan." },
  { "en": "Depresiasi?", "id": "Penyusutan nilai." },
  { "en": "Deprok?", "id": "Duduk di tanah." },
  { "en": "Depsid?", "id": "Senyawa." },
  { "en": "Depta?", "id": "Bagian (arkais)." },
  { "en": "Depun?", "id": "Kumpul (arkais)." },
  { "en": "Deputi?", "id": "Wakil." },
  { "en": "Dera?", "id": "Pukulan." },
  { "en": "Deragem?", "id": "Tiruan bunyi." },
  { "en": "Derai?", "id": "Butir." },
  { "en": "Derajat?", "id": "Tingkat." },
  { "en": "Derak?", "id": "Bunyi retak." },
  { "en": "Deram?", "id": "Bunyi gemuruh." },
  { "en": "Deran?", "id": "Takut (arkais)." },
  { "en": "Derang?", "id": "Bunyi nyaring." },
  { "en": "Deras?", "id": "Cepat (aliran)." },
  { "en": "Derat?", "id": "Bunyi kecil." },
  { "en": "Deregulasi?", "id": "Pengurangan aturan." },
  { "en": "Derek?", "id": "Alat angkat." },
  { "en": "Derel?", "id": "Lepas rel." },
  { "en": "Dereliksi?", "id": "Pengabaian." },
  { "en": "Deres?", "id": "Sadap." },
  { "en": "Deret?", "id": "Baris." },
  { "en": "Dergama?", "id": "Singa (arkais)." },
  { "en": "Derik?", "id": "Bunyi jangkrik." },
  { "en": "Dering?", "id": "Bunyi telepon." },
  { "en": "Deris?", "id": "Bunyi gesekan." },
  { "en": "Derit?", "id": "Bunyi pintu." },
  { "en": "Derita?", "id": "Sengsara." },
  { "en": "Derivasi?", "id": "Penurunan." },
  { "en": "Derivat?", "id": "Turunan." },
  { "en": "Derji?", "id": "Penjahit." },
  { "en": "Derma?", "id": "Sumbangan." },
  { "en": "Dermaga?", "id": "Pelabuhan." },
  { "en": "Dermal?", "id": "Kulit." },
  { "en": "Dermatitis?", "id": "Radang kulit." },
  { "en": "Dermatolog?", "id": "Ahli kulit." },
  { "en": "Dermatologi?", "id": "Ilmu kulit." },
  { "en": "Dermatom?", "id": "Alat iris kulit." },
  { "en": "Dermoid?", "id": "Mirip kulit." },
  { "en": "Deronasasi?", "id": "Pelunturan cat." },
  { "en": "Deru?", "id": "Gemuruh." },
  { "en": "Deruk?", "id": "Bunyi." },
  { "en": "Derum?", "id": "Bunyi mesin." },
  { "en": "Derung?", "id": "Bunyi." },
  { "en": "Desa?", "id": "Kampung." },
  { "en": "Desak?", "id": "Dorong." },
  { "en": "Desain?", "id": "Rancangan." },
  { "en": "Desainer?", "id": "Perancang." },
  { "en": "Desakralisasi?", "id": "Penghilangan sakral." },
  { "en": "Desalinasi?", "id": "Penghilangan garam." },
  { "en": "Desar?", "id": "Bunyi." },
  { "en": "Desember?", "id": "Bulan keduabelas." },
  { "en": "Desensi?", "id": "Penurunan." },
  { "en": "Desentralisasi?", "id": "Penyebaran wewenang." },
  { "en": "Desertir?", "id": "Pembelot." },
  { "en": "Desersi?", "id": "Pembelotan." },
  { "en": "Desibel?", "id": "Satuan suara." },
  { "en": "Desidua?", "id": "Selaput rahim." },
  { "en": "Desih?", "id": "Bujukan." },
  { "en": "Desik?", "id": "Bunyi." },
  { "en": "Desikan?", "id": "Pengering." },
  { "en": "Desikator?", "id": "Alat pengering." },
  { "en": "Desiliter?", "id": "Satuan volume." },
  { "en": "Desimal?", "id": "Persepuluhan." },
  { "en": "Desimeter?", "id": "Satuan panjang." },
  { "en": "Desinfeksi?", "id": "Pembasmian kuman." },
  { "en": "Desinfektan?", "id": "Pembasmi kuman." },
  { "en": "Desing?", "id": "Bunyi peluru." },
  { "en": "Desir?", "id": "Bunyi angin." },
  { "en": "Desis?", "id": "Bunyi ular." },
  { "en": "Desit?", "id": "Bunyi (arkais)." },
  { "en": "Deskripsi?", "id": "Uraian." },
  { "en": "Deskriptif?", "id": "Menggambarkan." },
  { "en": "Desmonem?", "id": "Sel sengat." },
  { "en": "Desmoplasia?", "id": "Pertumbuhan jaringan." },
  { "en": "Desmosom?", "id": "Struktur sel." },
  { "en": "Despot?", "id": "Tiran." },
  { "en": "Despotis?", "id": "Tirani." },
  { "en": "Despotisme?", "id": "Pemerintahan tirani." },
  { "en": "Destar?", "id": "Ikat kepala." },
  { "en": "Destinasi?", "id": "Tujuan." },
  { "en": "Destruksi?", "id": "Perusakan." },
  { "en": "Destruktif?", "id": "Merusak." },
  { "en": "Destruktor?", "id": "Perusak." },
  { "en": "Desuk?", "id": "Bunyi." },
  { "en": "Desup?", "id": "Bunyi." },
  { "en": "Desur?", "id": "Bunyi." },
  { "en": "Desus?", "id": "Kabar angin." },
  { "en": "Detail?", "id": "Rinci." },
  { "en": "Detak?", "id": "Denyut." },
  { "en": "Detap?", "id": "Bunyi." },
  { "en": "Detar?", "id": "Getar." },
  { "en": "Detas?", "id": "Bunyi letusan kecil." },
  { "en": "Detasemen?", "id": "Satuan tentara." },
  { "en": "Deteksi?", "id": "Pelacakan." },
  { "en": "Detektif?", "id": "Penyelidik." },
  { "en": "Detektor?", "id": "Alat pendeteksi." },
  { "en": "Detenidos?", "id": "Tahanan (Spanyol)." },
  { "en": "Detensi?", "id": "Penahanan." },
  { "en": "Detergen?", "id": "Sabun cuci." },
  { "en": "Deteriorasi?", "id": "Kemunduran." },
  { "en": "Determinisme?", "id": "Paham serba pasti." },
  { "en": "Detik?", "id": "Satuan waktu." },
  { "en": "Deting?", "id": "Bunyi." },
  { "en": "Detoksifikasi?", "id": "Penghilangan racun." },
  { "en": "Detonasi?", "id": "Ledakan." },
  { "en": "Detonator?", "id": "Pemicu ledakan." },
  { "en": "Detritus?", "id": "Sisa hancuran." },
  { "en": "Deus?", "id": "Tuhan (Latin)." },
  { "en": "Deuterium?", "id": "Isotop hidrogen." },
  { "en": "Deuteron?", "id": "Inti deuterium." },
  { "en": "Deva?", "id": "Dewa (Sanskerta)." },
  { "en": "Devaluasi?", "id": "Penurunan nilai mata uang." },
  { "en": "Developer?", "id": "Pengembang (Inggris)." },
  { "en": "Devisa?", "id": "Alat pembayaran luar negeri." },
  { "en": "Devosi?", "id": "Ketaatan agama." },
  { "en": "Dewa?", "id": "Tuhan." },
  { "en": "Dewadaru?", "id": "Pohon." },
  { "en": "Dewala?", "id": "Pencuri (arkais)." },
  { "en": "Dewan?", "id": "Majelis." },
  { "en": "Dewangga?", "id": "Kain sutra." },
  { "en": "Dewasa?", "id": "Matang." },
  { "en": "Dewata?", "id": "Para dewa." },
  { "en": "Dewi?", "id": "Dewi." },
  { "en": "Di?", "id": "Pada." },
  { "en": "Dia?", "id": "Ia." },
  { "en": "Diabetes?", "id": "Penyakit gula." },
  { "en": "Diadem?", "id": "Mahkota." },
  { "en": "Diafragma?", "id": "Sekat rongga badan." },
  { "en": "Diagnosis?", "id": "Penentuan penyakit." },
  { "en": "Diagnostik?", "id": "Berkaitan diagnosis." },
  { "en": "Diagonal?", "id": "Miring." },
  { "en": "Diagram?", "id": "Bagan." },
  { "en": "Diaken?", "id": "Jabatan gereja." },
  { "en": "Dialek?", "id": "Logat daerah." },
  { "en": "Dialektika?", "id": "Seni berdebat." },
  { "en": "Dialog?", "id": "Percakapan." },
  { "en": "Diam?", "id": "Senyap." },
  { "en": "Diameter?", "id": "Garis tengah." },
  { "en": "Dian?", "id": "Pelita." },
  { "en": "Diaper?", "id": "Popok." },
  { "en": "Diare?", "id": "Menceret." },
  { "en": "Diaspora?", "id": "Perantauan." },
  { "en": "Diastase?", "id": "Enzim." },
  { "en": "Diastole?", "id": "Relaksasi jantung." },
  { "en": "Diat?", "id": "Siap (arkais)." },
  { "en": "Diatermi?", "id": "Pemanasan jaringan." },
  { "en": "Diatesis?", "id": "Kecenderungan penyakit." },
  { "en": "Diatomea?", "id": "Alga." },
  { "en": "Diatonik?", "id": "Skala musik." },
  { "en": "Dida?", "id": "Ayah (arkais)." },
  { "en": "Didaktik?", "id": "Bersifat mengajar." },
  { "en": "Didaktikus?", "id": "Ahli mengajar." },
  { "en": "Didih?", "id": "Mendidih." },
  { "en": "Didik?", "id": "Ajar." },
  { "en": "Didis?", "id": "Asap (arkais)." },
  { "en": "Diesel?", "id": "Mesin." },
  { "en": "Diet?", "id": "Pengaturan makan." },
  { "en": "Diferensial?", "id": "Berkaitan perbedaan." },
  { "en": "Diferensiasi?", "id": "Pembedaan." },
  { "en": "Difraksi?", "id": "Penyebaran gelombang." },
  { "en": "Difteri?", "id": "Penyakit." },
  { "en": "Difusi?", "id": "Penyebaran." },
  { "en": "Digdaya?", "id": "Sakti." },
  { "en": "Digestif?", "id": "Pencernaan." },
  { "en": "Digit?", "id": "Angka." },
  { "en": "Digital?", "id": "Berkaitan angka." },
  { "en": "Diglosia?", "id": "Dua bahasa." },
  { "en": "Digraf?", "id": "Dua huruf satu bunyi." },
  { "en": "Dih?", "id": "Desa (Persia)." },
  { "en": "Dihidroksi?", "id": "Senyawa kimia." },
  { "en": "Dik?", "id": "Adik." },
  { "en": "Dikit?", "id": "Sedikit." },
  { "en": "Diksi?", "id": "Pilihan kata." },
  { "en": "Diktat?", "id": "Bahan kuliah." },
  { "en": "Diktator?", "id": "Penguasa absolut." },
  { "en": "Dikte?", "id": "Imla." },
  { "en": "Diktum?", "id": "Putusan." },
  { "en": "Dil?", "id": "Tanaman." },
  { "en": "Dila?", "id": "Lidah (arkais)." },
  { "en": "Dilak?", "id": "Jilat (arkais)." },
  { "en": "Dilam?", "id": "Nilam." },
  { "en": "Dilema?", "id": "Pilihan sulit." },
  { "en": "Diler?", "id": "Penyalur." },
  { "en": "Diligensi?", "id": "Ketekunan." },
  { "en": "Dim?", "id": "Redup." },
  { "en": "Dimensi?", "id": "Ukuran." },
  { "en": "Dimer?", "id": "Molekul." },
  { "en": "Dimorfik?", "id": "Dua bentuk." },
  { "en": "Din?", "id": "Agama (Arab)." },
  { "en": "Dina?", "id": "Hina." },
  { "en": "Dinamika?", "id": "Gerak." },
  { "en": "Dinamis?", "id": "Bergerak." },
  { "en": "Dinamit?", "id": "Bahan peledak." },
  { "en": "Dinamo?", "id": "Pembangkit listrik." },
  { "en": "Dinar?", "id": "Mata uang." },
  { "en": "Dinas?", "id": "Jawatan." },
  { "en": "Dinasti?", "id": "Keturunan raja." },
  { "en": "Dinding?", "id": "Tembok." },
  { "en": "Dingin?", "id": "Sejuk." },
  { "en": "Dingkis?", "id": "Kurus (arkais)." },
  { "en": "Dingkit?", "id": "Berjinjit." },
  { "en": "Dingo?", "id": "Anjing liar Australia." },
  { "en": "Dini?", "id": "Pagi sekali." },
  { "en": "Dinihari?", "id": "Subuh." },
  { "en": "Diniah?", "id": "Keagamaan." },
  { "en": "Dioda?", "id": "Komponen elektronika." },
  { "en": "Diopsida?", "id": "Mineral." },
  { "en": "Dioptri?", "id": "Ukuran lensa." },
  { "en": "Diorama?", "id": "Miniatur tiga dimensi." },
  { "en": "Diorit?", "id": "Batuan." },
  { "en": "Dipa?", "id": "Pelita (arkais)." },
  { "en": "Dipan?", "id": "Tempat tidur." },
  { "en": "Diploma?", "id": "Ijazah." },
  { "en": "Diplomasi?", "id": "Perundingan." },
  { "en": "Diplomat?", "id": "Ahli diplomasi." },
  { "en": "Diplomatik?", "id": "Berkaitan diplomasi." },
  { "en": "Dipnoi?", "id": "Ikan paru." },
  { "en": "Dipol?", "id": "Dua kutub." },
  { "en": "Diptera?", "id": "Ordo serangga." },
  { "en": "Diptotos?", "id": "Kata dua kasus." },
  { "en": "Dirah?", "id": "Perisai (arkais)." },
  { "en": "Diraja?", "id": "Kerajaan." },
  { "en": "Dirgahayu?", "id": "Panjang umur." },
  { "en": "Dirham?", "id": "Mata uang." },
  { "en": "Diri?", "id": "Pribadi." },
  { "en": "Dirigen?", "id": "Pemimpin orkes." },
  { "en": "Diris?", "id": "Siram." },
  { "en": "Dirit?", "id": "Tulis (arkais)." },
  { "en": "Dirjam?", "id": "Rantai (arkais)." },
  { "en": "Dirjen?", "id": "Direktur jenderal." },
  { "en": "Dirus?", "id": "Siram." },
  { "en": "Disagio?", "id": "Selisih kurs." },
  { "en": "Disain?", "id": "Desain." },
  { "en": "Disakarida?", "id": "Gula." },
  { "en": "Disastria?", "id": "Gangguan bicara." },
  { "en": "Disbursemen?", "id": "Pembayaran." },
  { "en": "Disdrometer?", "id": "Pengukur hujan." },
  { "en": "Disel?", "id": "Diesel." },
  { "en": "Disensus?", "id": "Ketidaksetujuan." },
  { "en": "Disentri?", "id": "Menceret darah." },
  { "en": "Disertasi?", "id": "Karya tulis doktor." },
  { "en": "Disfagia?", "id": "Sulit menelan." },
  { "en": "Disfasia?", "id": "Gangguan bahasa." },
  { "en": "Disfemisme?", "id": "Ungkapan kasar." },
  { "en": "Disfungsi?", "id": "Gangguan fungsi." },
  { "en": "Disgenik?", "id": "Penurunan mutu gen." },
  { "en": "Disharmoni?", "id": "Ketidakselarasan." },
  { "en": "Disiden?", "id": "Pembangkang." },
  { "en": "Disilabik?", "id": "Dua suku kata." },
  { "en": "Disimilasi?", "id": "Perbedaan." },
  { "en": "Disinfeksi?", "id": "Pembasmian kuman." },
  { "en": "Disinformasi?", "id": "Informasi salah." },
  { "en": "Disinsentif?", "id": "Penghambat." },
  { "en": "Disintegrasi?", "id": "Perpecahan." },
  { "en": "Disiplin?", "id": "Tata tertib." },
  { "en": "Disjoki?", "id": "DJ." },
  { "en": "Disjungsi?", "id": "Pemisahan." },
  { "en": "Diska?", "id": "Piringan." },
  { "en": "Disket?", "id": "Media simpan." },
  { "en": "Disko?", "id": "Musik dansa." },
  { "en": "Diskon?", "id": "Potongan harga." },
  { "en": "Diskontinu?", "id": "Tidak berlanjut." },
  { "en": "Diskonto?", "id": "Bunga." },
  { "en": "Diskredit?", "id": "Memojokkan." },
  { "en": "Diskrepansi?", "id": "Kesenjangan." },
  { "en": "Diskresi?", "id": "Kebijakan." },
  { "en": "Diskriminasi?", "id": "Pembedaan." },
  { "en": "Diskualifikasi?", "id": "Pencabutan hak." },
  { "en": "Diskulpasi?", "id": "Pembebasan tuduhan." },
  { "en": "Diskursif?", "id": "Analitis." },
  { "en": "Diskursus?", "id": "Wacana." },
  { "en": "Diskusi?", "id": "Pembahasan." },
  { "en": "Dislalia?", "id": "Cadel." },
  { "en": "Disleksia?", "id": "Sulit membaca." },
  { "en": "Dislokasi?", "id": "Pergeseran." },
  { "en": "Dismenorea?", "id": "Nyeri haid." },
  { "en": "Disolusi?", "id": "Pembubaran." },
  { "en": "Disonansi?", "id": "Ketidaksesuaian." },
  { "en": "Disoperasi?", "id": "Salah urus." },
  { "en": "Disorganisasi?", "id": "Kekacauan." },
  { "en": "Disorientasi?", "id": "Bingung arah." },
  { "en": "Disosiasi?", "id": "Pemisahan." },
  { "en": "Dispareunia?", "id": "Nyeri sanggama." },
  { "en": "Disparitas?", "id": "Perbedaan." },
  { "en": "Dispnea?", "id": "Sesak napas." },
  { "en": "Disposisi?", "id": "Penempatan." },
  { "en": "Distal?", "id": "Jauh." },
  { "en": "Distansi?", "id": "Jarak." },
  { "en": "Distikiasis?", "id": "Bulu mata ganda." },
  { "en": "Distikon?", "id": "Sajak dua baris." },
  { "en": "Distilasi?", "id": "Penyulingan." },
  { "en": "Distingtif?", "id": "Khas." },
  { "en": "Distorsi?", "id": "Penyimpangan." },
  { "en": "Distribusi?", "id": "Penyaluran." },
  { "en": "Distributor?", "id": "Penyalur." },
  { "en": "Distrik?", "id": "Daerah." },
  { "en": "Disuasi?", "id": "Pencegahan." },
  { "en": "Dita?", "id": "Lihat (arkais)." },
  { "en": "Dite?", "id": "Lagu (Jerman)." },
  { "en": "Diter?", "id": "Nama tumbuhan." },
  { "en": "Ditirambe?", "id": "Pujian berlebihan." },
  { "en": "Dito?", "id": "Jari (arkais)." },
  { "en": "Ditransitif?", "id": "Membutuhkan dua objek." },
  { "en": "Diuresis?", "id": "Peningkatan kencing." },
  { "en": "Diuretik?", "id": "Peluruh kencing." },
  { "en": "Diurnal?", "id": "Aktif siang hari." },
  { "en": "Divergen?", "id": "Menyebar." },
  { "en": "Divergensi?", "id": "Penyebaran." },
  { "en": "Diversifikasi?", "id": "Penganekaragaman." },
  { "en": "Diversitas?", "id": "Keanekaragaman." },
  { "en": "Dividen?", "id": "Bagi hasil." },
  { "en": "Divisi?", "id": "Bagian." },
  { "en": "Doa?", "id": "Permohonan." },
  { "en": "Doang?", "id": "Saja (prokem)." },
  { "en": "Dobel?", "id": "Ganda." },
  { "en": "Doble?", "id": "Dobel." },
  { "en": "Dobolo?", "id": "Permainan." },
  { "en": "Dobrak?", "id": "Rusak paksa." },
  { "en": "Doceng?", "id": "Lemah (arkais)." },
  { "en": "Dodet?", "id": "Luka kecil." },
  { "en": "Dodi?", "id": "Mainan anak." },
  { "en": "Dodok?", "id": "Duduk." },
  { "en": "Dodol?", "id": "Makanan manis." },
  { "en": "Dodot?", "id": "Kain." },
  { "en": "Doeloe?", "id": "Dulu (ejaan lama)." },
  { "en": "Dog?", "id": "Anjing (Inggris)." },
  { "en": "Dogel?", "id": "Pendek (ekor)." },
  { "en": "Doger?", "id": "Tarian." },
  { "en": "Dogma?", "id": "Ajaran mutlak." },
  { "en": "Dogmatik?", "id": "Bersifat dogma." },
  { "en": "Dogmatis?", "id": "Menurut dogma." },
  { "en": "Dogo?", "id": "Anjing besar." },
  { "en": "Dohok?", "id": "Serakah (arkais)." },
  { "en": "Doku?", "id": "Uang (prokem)." },
  { "en": "Dokar?", "id": "Kereta kuda." },
  { "en": "Dokoh?", "id": "Perhiasan leher." },
  { "en": "Dokter?", "id": "Tabib." },
  { "en": "Doktor?", "id": "Gelar akademik." },
  { "en": "Doktrin?", "id": "Ajaran." },
  { "en": "Dokumen?", "id": "Surat penting." },
  { "en": "Dokumentasi?", "id": "Pengumpulan dokumen." },
  { "en": "Dol?", "id": "Gila (arkais)." },
  { "en": "Dolan?", "id": "Jalan-jalan (Jawa)." },
  { "en": "Dolar?", "id": "Mata uang." },
  { "en": "Doldrum?", "id": "Daerah tenang (laut)." },
  { "en": "Dolmen?", "id": "Meja batu purba." },
  { "en": "Dolomit?", "id": "Mineral." },
  { "en": "Dom?", "id": "Gereja katedral (Belanda)." },
  { "en": "Domain?", "id": "Wilayah." },
  { "en": "Domba?", "id": "Biri-biri." },
  { "en": "Domein?", "id": "Wilayah (Belanda)." },
  { "en": "Domestik?", "id": "Dalam negeri." },
  { "en": "Dominan?", "id": "Unggul." },
  { "en": "Dominasi?", "id": "Penguasaan." },
  { "en": "Domine?", "id": "Pendeta (Belanda)." },
  { "en": "Domini?", "id": "Tahun Tuhan (Latin)." },
  { "en": "Dominion?", "id": "Wilayah kekuasaan." },
  { "en": "Domino?", "id": "Permainan." },
  { "en": "Domisili?", "id": "Tempat tinggal." },
  { "en": "Domot?", "id": "Tumpul (arkais)." },
  { "en": "Dompak?", "id": "Lompat." },
  { "en": "Dompet?", "id": "Wadah uang." },
  { "en": "Domplang?", "id": "Tidak seimbang." },
  { "en": "Dompleng?", "id": "Ikut serta." },
  { "en": "Donasi?", "id": "Sumbangan." },
  { "en": "Donat?", "id": "Kue." },
  { "en": "Donatur?", "id": "Penyumbang." },
  { "en": "Dondang?", "id": "Ayunan." },
  { "en": "Dong?", "id": "Partikel penegas." },
  { "en": "Dongak?", "id": "Tengadah." },
  { "en": "Dongan?", "id": "Teman (Batak)." },
  { "en": "Dongbret?", "id": "Alat musik." },
  { "en": "Dongeng?", "id": "Cerita khayal." },
  { "en": "Dongkak?", "id": "Lompat (arkais)." },
  { "en": "Dongkel?", "id": "Cungkil." },
  { "en": "Dongkok?", "id": "Dekap (arkais)." },
  { "en": "Dongkol?", "id": "Jengkel." },
  { "en": "Dongkrak?", "id": "Alat pengungkit." },
  { "en": "Donor?", "id": "Penderma." },
  { "en": "Donto?", "id": "Bodoh (Jawa)." },
  { "en": "Dop?", "id": "Penutup." },
  { "en": "Doping?", "id": "Obat perangsang." },
  { "en": "Dorna?", "id": "Tokoh wayang." },
  { "en": "Dorong?", "id": "Tolak." },
  { "en": "Dorsal?", "id": "Bagian punggung." },
  { "en": "Dos?", "id": "Kardus." },
  { "en": "Dosa?", "id": "Kesalahan." },
  { "en": "Dosen?", "id": "Pengajar perguruan tinggi." },
  { "en": "Doser?", "id": "Alat berat." },
  { "en": "Dosis?", "id": "Takaran." },
  { "en": "Dot?", "id": "Puting susu botol." },
  { "en": "Doyak?", "id": "Goyang (arkais)." },
  { "en": "Doyan?", "id": "Suka." },
  { "en": "Doyong?", "id": "Miring." },
  { "en": "Draf?", "id": "Rancangan." },
  { "en": "Drainase?", "id": "Saluran air." },
  { "en": "Drakula?", "id": "Vampir." },
  { "en": "Drama?", "id": "Sandiwara." },
  { "en": "Dramatik?", "id": "Mengharukan." },
  { "en": "Dramatis?", "id": "Dramatik." },
  { "en": "Dramaturgi?", "id": "Seni drama." },
  { "en": "Drap?", "id": "Kain penutup." },
  { "en": "Drastis?", "id": "Mencolok." },
  { "en": "Drawa?", "id": "Lelucon (arkais)." },
  { "en": "Drel?", "id": "Bor." },
  { "en": "Dribel?", "id": "Giring bola." },
  { "en": "Dril?", "id": "Latihan." },
  { "en": "Drip?", "id": "Tetes (infus)." },
  { "en": "Drop?", "id": "Jatuh." },
  { "en": "Drum?", "id": "Genderang." },
  { "en": "Druwe?", "id": "Milik (Jawa)." },
  { "en": "Dua?", "id": "Angka." },
  { "en": "Dual?", "id": "Ganda." },
  { "en": "Dualis?", "id": "Bersifat ganda." },
  { "en": "Dualisme?", "id": "Paham serba dua." },
  { "en": "Dubalang?", "id": "Hulubalang." },
  { "en": "Dubius?", "id": "Meragukan." },
  { "en": "Dublir?", "id": "Sulih suara." },
  { "en": "Dubur?", "id": "Anus." },
  { "en": "Duda?", "id": "Pria tanpa istri." },
  { "en": "Duduk?", "id": "Bersemayam." },
  { "en": "Dudur?", "id": "Belalang (arkais)." },
  { "en": "Duel?", "id": "Perkelahian satu lawan satu." },
  { "en": "Duet?", "id": "Nyanyian berdua." },
  { "en": "Duga?", "id": "Kira." },
  { "en": "Dugang?", "id": "Sangga." },
  { "en": "Duhai?", "id": "Wahai." },
  { "en": "Duit?", "id": "Uang." },
  { "en": "Duka?", "id": "Sedih." },
  { "en": "Dukacarita?", "id": "Kisah sedih." },
  { "en": "Dukacita?", "id": "Kesedihan." },
  { "en": "Dukan?", "id": "Toko (arkais)." },
  { "en": "Dukana?", "id": "Sedih (arkais)." },
  { "en": "Dukat?", "id": "Mata uang emas." },
  { "en": "Duku?", "id": "Buah." },
  { "en": "Dukuh?", "id": "Kampung kecil." },
  { "en": "Dukun?", "id": "Tabib tradisional." },
  { "en": "Dukung?", "id": "Sokong." },
  { "en": "Dulag?", "id": "Beduk." },
  { "en": "Dulang?", "id": "Nampan kayu." },
  { "en": "Duli?", "id": "Debu (kaki raja)." },
  { "en": "Dulu?", "id": "Lampau." },
  { "en": "Dum?", "id": "Peluru (arkais)." },
  { "en": "Dumdum?", "id": "Peluru." },
  { "en": "Dumpis?", "id": "Kecil (arkais)." },
  { "en": "Dumung?", "id": "Bodoh (arkais)." },
  { "en": "Dungas?", "id": "Sombong (arkais)." },
  { "en": "Dungkul?", "id": "Tidak bertanduk." },
  { "en": "Dungu?", "id": "Bodoh." },
  { "en": "Dunia?", "id": "Jagad." },
  { "en": "Duniawi?", "id": "Fana." },
  { "en": "Dup?", "id": "Tempat dupa." },
  { "en": "Dupa?", "id": "Kemenyan." },
  { "en": "Dupleks?", "id": "Rangkap dua." },
  { "en": "Duplikasi?", "id": "Penggandaan." },
  { "en": "Duplikat?", "id": "Salinan." },
  { "en": "Dur?", "id": "Jauh (arkais)." },
  { "en": "Dura?", "id": "Bertahan (arkais)." },
  { "en": "Durabel?", "id": "Tahan lama." },
  { "en": "Durasi?", "id": "Lama waktu." },
  { "en": "Durat?", "id": "Mutiara (Arab)." },
  { "en": "Duratif?", "id": "Menyatakan lama." },
  { "en": "Duren?", "id": "Durian." },
  { "en": "Durga?", "id": "Dewi (Hindu)." },
  { "en": "Durhaka?", "id": "Tidak taat." },
  { "en": "Duri?", "id": "Bagian tajam tumbuhan." },
  { "en": "Durian?", "id": "Buah." },
  { "en": "Durja?", "id": "Muka (Sanskerta)." },
  { "en": "Durjana?", "id": "Jahat." },
  { "en": "Durkarsa?", "id": "Niat jahat." },
  { "en": "Durna?", "id": "Licik (tokoh wayang)." },
  { "en": "Durno?", "id": "Durna." },
  { "en": "Dursila?", "id": "Jahat." },
  { "en": "Duru?", "id": "Empedu (arkais)." },
  { "en": "Duruk?", "id": "Duduk berkumpul." },
  { "en": "Durus?", "id": "Pelajaran (Arab)." },
  { "en": "Dus?", "id": "Maka." },
  { "en": "Dusta?", "id": "Bohong." },
  { "en": "Dusun?", "id": "Kampung." },
  { "en": "Duta?", "id": "Utusan." },
  { "en": "Duwet?", "id": "Jamblang." },
  { "en": "Duyung?", "id": "Ikan." },
  { "en": "Eba?", "id": "Arah (arkais)." },
  { "en": "Eban?", "id": "Kayu hitam." },
  { "en": "Ebang?", "id": "Azan (arkais)." },
  { "en": "Ebi?", "id": "Udang kering." },
  { "en": "Ebonit?", "id": "Karet keras." },
  { "en": "Ebro?", "id": "Wadah air." },
  { "en": "Ebros?", "id": "Kuas." },
  { "en": "Eburina?", "id": "Zat gading." },
  { "en": "Ecek?", "id": "Pura-pura (prokem)." },
  { "en": "Ecer?", "id": "Jual satuan." },
  { "en": "Eces?", "id": "Mudah (arkais)." },
  { "en": "Eda?", "id": "Ayah (Batak)." },
  { "en": "Edan?", "id": "Gila (Jawa)." },
  { "en": "Edar?", "id": "Keling." },
  { "en": "Edisi?", "id": "Terbitan." },
  { "en": "Edit?", "id": "Sunting." },
  { "en": "Editor?", "id": "Penyunting." },
  { "en": "Editorial?", "id": "Tajuk rencana." },
  { "en": "Edukasi?", "id": "Pendidikan." },
  { "en": "Edukatif?", "id": "Mendidik." },
  { "en": "Efek?", "id": "Akibat." },
  { "en": "Efektif?", "id": "Manjur." },
  { "en": "Efendi?", "id": "Tuan (Turki)." },
  { "en": "Efisien?", "id": "Sangkil." },
  { "en": "Efloresensi?", "id": "Pengkristalan." },
  { "en": "Efusi?", "id": "Pancaran." },
  { "en": "Egal?", "id": "Sama." },
  { "en": "Egalitarian?", "id": "Paham kesamaan." },
  { "en": "Egalitarisme?", "id": "Egalitarian." },
  { "en": "Ego?", "id": "Aku." },
  { "en": "Egois?", "id": "Mementingkan diri." },
  { "en": "Egoisme?", "id": "Sikap egois." },
  { "en": "Egosentris?", "id": "Berpusat pada diri." },
  { "en": "Egomaniak?", "id": "Gila diri." },
  { "en": "Egrang?", "id": "Permainan." },
  { "en": "Egresif?", "id": "Bersifat keluar." },
  { "en": "Eh?", "id": "Seruan." },
  { "en": "Ehem?", "id": "Deham." },
  { "en": "Eidetik?", "id": "Imaji jelas." },
  { "en": "Eigendom?", "id": "Hak milik (Belanda)." },
  { "en": "Eja?", "id": "Lafalkan huruf." },
  { "en": "Ejaan?", "id": "Cara menulis." },
  { "en": "Ejek?", "id": "Olok." },
  { "en": "Ejektor?", "id": "Alat pelontar." },
  { "en": "Eka?", "id": "Satu." },
  { "en": "Ekajati?", "id": "Kelahiran pertama (Hindu)." },
  { "en": "Ekakarsa?", "id": "Satu kehendak." },
  { "en": "Ekamatra?", "id": "Satu dimensi." },
  { "en": "Ekang?", "id": "Renggang (arkais)." },
  { "en": "Ekaristi?", "id": "Sakramen Kristen." },
  { "en": "Eke?", "id": "Aku (Betawi)." },
  { "en": "Ekel?", "id": "Jijik (Jawa)." },
  { "en": "Ekidin?", "id": "Raja (arkais)." },
  { "en": "Eklektik?", "id": "Memilih dari berbagai sumber." },
  { "en": "Eklektisisme?", "id": "Paham eklektik." },
  { "en": "Eklips?", "id": "Gerhana." },
  { "en": "Ekliptika?", "id": "Jalur edar." },
  { "en": "Ekofraksia?", "id": "Meniru ucapan." },
  { "en": "Ekologi?", "id": "Ilmu lingkungan." },
  { "en": "Ekon?", "id": "Kain (arkais)." },
  { "en": "Ekonomi?", "id": "Tata kelola keuangan." },
  { "en": "Ekonomis?", "id": "Hemat." },
  { "en": "Ekor?", "id": "Bagian belakang." },
  { "en": "Ekosistem?", "id": "Tata lingkungan." },
  { "en": "Ekrab?", "id": "Kalajengking (Arab)." },
  { "en": "Ekran?", "id": "Layar." },
  { "en": "Eks?", "id": "Bekas." },
  { "en": "Eksak?", "id": "Pasti." },
  { "en": "Eksakta?", "id": "Ilmu pasti." },
  { "en": "Eksaltasi?", "id": "Peninggian." },
  { "en": "Eksamen?", "id": "Ujian." },
  { "en": "Eksaminasi?", "id": "Pengujian." },
  { "en": "Eksaminator?", "id": "Penguji." },
  { "en": "Eksantem?", "id": "Ruam kulit." },
  { "en": "Eksegesis?", "id": "Penafsiran kitab." },
  { "en": "Eksekusi?", "id": "Pelaksanaan." },
  { "en": "Eksekutif?", "id": "Pelaksana." },
  { "en": "Eksekutor?", "id": "Pelaksana." },
  { "en": "Eksem?", "id": "Penyakit kulit." },
  { "en": "Eksemplar?", "id": "Lembar." },
  { "en": "Eksentrik?", "id": "Aneh." },
  { "en": "Eksepsi?", "id": "Pengecualian." },
  { "en": "Ekses?", "id": "Kelebihan." },
  { "en": "Eksfoliasi?", "id": "Pengelupasan." },
  { "en": "Ekshalasi?", "id": "Pengembusan napas." },
  { "en": "Ekshibisi?", "id": "Pameran." },
  { "en": "Ekshibisionis?", "id": "Suka pamer." },
  { "en": "Eksikator?", "id": "Desikator." },
  { "en": "Eksil?", "id": "Orang buangan." },
  { "en": "Eksipien?", "id": "Zat tambahan obat." },
  { "en": "Eksis?", "id": "Ada." },
  { "en": "Eksistensi?", "id": "Keberadaan." },
  { "en": "Eksistensialisme?", "id": "Paham filsafat." },
  { "en": "Eksitus?", "id": "Keluaran." },
  { "en": "Ekskavasi?", "id": "Penggalian." },
  { "en": "Eksklave?", "id": "Wilayah terpisah." },
  { "en": "Eksklusif?", "id": "Khusus." },
  { "en": "Ekskresi?", "id": "Pengeluaran sisa." },
  { "en": "Ekskreta?", "id": "Hasil ekskresi." },
  { "en": "Ekskulpasi?", "id": "Pembebasan tuntutan." },
  { "en": "Ekskursi?", "id": "Perjalanan." },
  { "en": "Eksobiologi?", "id": "Ilmu kehidupan luar angkasa." },
  { "en": "Eksodermis?", "id": "Lapisan luar." },
  { "en": "Eksodos?", "id": "Keluaran besar-besaran." },
  { "en": "Eksodus?", "id": "Eksodos." },
  { "en": "Eksoergik?", "id": "Melepas energi." },
  { "en": "Eksofasia?", "id": "Bicara lantang." },
  { "en": "Eksofora?", "id": "Menunjuk keluar teks." },
  { "en": "Eksogam?", "id": "Kawin luar suku." },
  { "en": "Eksogami?", "id": "Praktik eksogam." },
  { "en": "Eksogen?", "id": "Berasal dari luar." },
  { "en": "Eksordium?", "id": "Pendahuluan pidato." },
  { "en": "Eksorsis?", "id": "Pengusir roh jahat." },
  { "en": "Eksosfer?", "id": "Lapisan atmosfer." },
  { "en": "Eksostosis?", "id": "Tonjolan tulang." },
  { "en": "Eksot?", "id": "Eksotis." },
  { "en": "Eksoterik?", "id": "Untuk umum." },
  { "en": "Eksotermik?", "id": "Melepas panas." },
  { "en": "Eksotik?", "id": "Unik." },
  { "en": "Eksotis?", "id": "Eksotik." },
  { "en": "Ekspansi?", "id": "Perluasan." },
  { "en": "Ekspansif?", "id": "Bersifat meluas." },
  { "en": "Ekspatriasi?", "id": "Pembuangan." },
  { "en": "Ekspatriat?", "id": "Pekerja asing." },
  { "en": "Ekspedisi?", "id": "Pengiriman." },
  { "en": "Eksper?", "id": "Ahli." },
  { "en": "Eksperimen?", "id": "Percobaan." },
  { "en": "Ekspirasi?", "id": "Pengeluaran napas." },
  { "en": "Eksplan?", "id": "Bagian tanaman." },
  { "en": "Eksplikasi?", "id": "Penjelasan." },
  { "en": "Eksplisit?", "id": "Jelas." },
  { "en": "Eksplorasi?", "id": "Penjelajahan." },
  { "en": "Eksploratif?", "id": "Bersifat menjelajah." },
  { "en": "Eksplosi?", "id": "Ledakan." },
  { "en": "Eksplosif?", "id": "Mudah meledak." },
  { "en": "Ekspo?", "id": "Pameran." },
  { "en": "Eksponen?", "id": "Pangkat." },
  { "en": "Ekspor?", "id": "Pengiriman ke luar negeri." },
  { "en": "Ekstasi?", "id": "Kegembiraan luar biasa." },
  { "en": "Ekstemporan?", "id": "Tanpa persiapan." },
  { "en": "Ekstensi?", "id": "Perpanjangan." },
  { "en": "Ekstensif?", "id": "Luas." },
  { "en": "Ekstensor?", "id": "Otot perenggang." },
  { "en": "Eksterior?", "id": "Bagian luar." },
  { "en": "Eksteritorialitas?", "id": "Hak khusus di luar wilayah." },
  { "en": "Ekstra?", "id": "Tambahan." },
  { "en": "Ekstradisi?", "id": "Penyerahan buronan." },
  { "en": "Ekstrak?", "id": "Sari." },
  { "en": "Ekstraksi?", "id": "Pengeluaran." },
  { "en": "Ekstrakurikuler?", "id": "Di luar kurikulum." },
  { "en": "Ekstralinguistik?", "id": "Di luar bahasa." },
  { "en": "Ekstramarital?", "id": "Di luar nikah." },
  { "en": "Ekstranei?", "id": "Orang asing." },
  { "en": "Ekstraparlementer?", "id": "Di luar parlemen." },
  { "en": "Ekstrapolasi?", "id": "Perkiraan lanjut." },
  { "en": "Ekstraterestrial?", "id": "Luar bumi." },
  { "en": "Ekstrateritorial?", "id": "Di luar wilayah hukum." },
  { "en": "Ekstrauterin?", "id": "Di luar rahim." },
  { "en": "Ekstraversi?", "id": "Sikap terbuka." },
  { "en": "Ekstrem?", "id": "Keras." },
  { "en": "Ekstremis?", "id": "Orang ekstrem." },
  { "en": "Ekstremitas?", "id": "Ujung." },
  { "en": "Ekstrinsik?", "id": "Berasal dari luar." },
  { "en": "Ektrusi?", "id": "Penyemburan." },
  { "en": "Ekuador?", "id": "Khatulistiwa." },
  { "en": "Ekuatif?", "id": "Menyatakan kesamaan." },
  { "en": "Ekuilibrium?", "id": "Keseimbangan." },
  { "en": "Ekuinoks?", "id": "Siang malam sama panjang." },
  { "en": "Ekuivalen?", "id": "Setara." },
  { "en": "Ekuiti?", "id": "Modal sendiri." },
  { "en": "Ekumene?", "id": "Gerakan kesatuan gereja." },
  { "en": "Ekumenis?", "id": "Bersifat ekumene." },
  { "en": "El?", "id": "Huruf." },
  { "en": "Ela?", "id": "Seruan." },
  { "en": "Elaborasi?", "id": "Penguraian rinci." },
  { "en": "Elan?", "id": "Semangat." },
  { "en": "Elang?", "id": "Burung pemangsa." },
  { "en": "Elastik?", "id": "Kenyal." },
  { "en": "Elastis?", "id": "Lentur." },
  { "en": "Elastisitas?", "id": "Kelenturan." },
  { "en": "Elastomer?", "id": "Polimer elastis." },
  { "en": "Elatif?", "id": "Menyatakan keluar." },
  { "en": "Elefantiasis?", "id": "Penyakit kaki gajah." },
  { "en": "Elefan?", "id": "Gajah (arkais)." },
  { "en": "Elegan?", "id": "Anggun." },
  { "en": "Elegansi?", "id": "Keanggunan." },
  { "en": "Elegi?", "id": "Sajak sedih." },
  { "en": "Elek?", "id": "Jelek (Jawa)." },
  { "en": "Eleksi?", "id": "Pemilihan." },
  { "en": "Elektif?", "id": "Dipilih." },
  { "en": "Elektrifikasi?", "id": "Pemasangan listrik." },
  { "en": "Elektrik?", "id": "Listrik." },
  { "en": "Elektris?", "id": "Berkaitan listrik." },
  { "en": "Elektrisitas?", "id": "Kelistrikan." },
  { "en": "Elektroda?", "id": "Kutub listrik." },
  { "en": "Elektrodinamika?", "id": "Ilmu gerak listrik." },
  { "en": "Elektroforesis?", "id": "Pemisahan partikel." },
  { "en": "Elektrokimia?", "id": "Ilmu kimia listrik." },
  { "en": "Elektrokusi?", "id": "Hukuman mati listrik." },
  { "en": "Elektrolisis?", "id": "Penguraian zat." },
  { "en": "Elektrolit?", "id": "Penghantar listrik." },
  { "en": "Elektromagnet?", "id": "Magnet listrik." },
  { "en": "Elektrometalurgi?", "id": "Pengolahan logam." },
  { "en": "Elektromiografi?", "id": "Perekaman aktivitas otot." },
  { "en": "Elektron?", "id": "Partikel atom." },
  { "en": "Elektronik?", "id": "Berkaitan elektron." },
  { "en": "Elektronika?", "id": "Ilmu elektron." },
  { "en": "Elektropatologi?", "id": "Penyakit akibat listrik." },
  { "en": "Elektroplating?", "id": "Penyepuhan." },
  { "en": "Elektroskop?", "id": "Alat deteksi muatan." },
  { "en": "Elektrostatika?", "id": "Ilmu listrik statis." },
  { "en": "Elektroteknik?", "id": "Teknik listrik." },
  { "en": "Elektrum?", "id": "Campuran emas perak." },
  { "en": "Elemen?", "id": "Unsur." },
  { "en": "Elementer?", "id": "Dasar." },
  { "en": "Elemi?", "id": "Resin." },
  { "en": "Elenchus?", "id": "Sanggahan (filsafat)." },
  { "en": "Eliksir?", "id": "Ramuan." },
  { "en": "Eliminasi?", "id": "Penyingkiran." },
  { "en": "Elips?", "id": "Lonjong." },
  { "en": "Elipsis?", "id": "Penghilangan kata." },
  { "en": "Elipsoid?", "id": "Bentuk elips." },
  { "en": "Elit?", "id": "Terpilih." },
  { "en": "Elite?", "id": "Elit." },
  { "en": "Elitis?", "id": "Bersifat elit." },
  { "en": "Elitisme?", "id": "Paham elitis." },
  { "en": "Elo?", "id": "Kamu (Jawa)." },
  { "en": "Elok?", "id": "Indah." },
  { "en": "Elokuensi?", "id": "Kefasihan berbicara." },
  { "en": "Elon?", "id": "Julur." },
  { "en": "Elongasi?", "id": "Pemanjangan." },
  { "en": "Elpiji?", "id": "Gas cair." },
  { "en": "Eltor?", "id": "Penyakit kolera." },
  { "en": "Elu?", "id": "Kamu (kasar)." },
  { "en": "Eluat?", "id": "Jijik." },
  { "en": "Eluen?", "id": "Pelarut." },
  { "en": "Elung?", "id": "Lekuk." },
  { "en": "Elus?", "id": "Usap." },
  { "en": "Elusi?", "id": "Penghindaran." },
  { "en": "Elusif?", "id": "Sulit dipahami." },
  { "en": "Elutriasi?", "id": "Pemisahan partikel." },
  { "en": "Eluviasi?", "id": "Pencucian tanah." },
  { "en": "Eluvium?", "id": "Endapan." },
  { "en": "Email?", "id": "Surel." },
  { "en": "Emanasi?", "id": "Pancaran." },
  { "en": "Emang?", "id": "Memang (prokem)." },
  { "en": "Emansipasi?", "id": "Persamaan hak." },
  { "en": "Emar?", "id": "Luka memar (arkais)." },
  { "en": "Emas?", "id": "Logam mulia." },
  { "en": "Embacang?", "id": "Buah." },
  { "en": "Embah?", "id": "Kakek/Nenek (Jawa)." },
  { "en": "Embal?", "id": "Lembap." },
  { "en": "Embalase?", "id": "Kemasan." },
  { "en": "Embalsamasi?", "id": "Pengawetan mayat." },
  { "en": "Embam?", "id": "Dekap (mulut)." },
  { "en": "Embargo?", "id": "Larangan." },
  { "en": "Embarkasi?", "id": "Keberangkatan." },
  { "en": "Embat?", "id": "Hantam." },
  { "en": "Embeles?", "id": "Bual." },
  { "en": "Embeli?", "id": "Sungai (arkais)." },
  { "en": "Embelia?", "id": "Tanaman." },
  { "en": "Ember?", "id": "Wadah air." },
  { "en": "Embes?", "id": "Lembap (Jawa)." },
  { "en": "Embet?", "id": "Angkat (arkais)." },
  { "en": "Embik?", "id": "Suara kambing." },
  { "en": "Embih?", "id": "Nenek buyut (Sunda)." },
  { "en": "Emblem?", "id": "Lambang." },
  { "en": "Embok?", "id": "Ibu (Jawa)." },
  { "en": "Embol?", "id": "Sumbat." },
  { "en": "Emboli?", "id": "Sumbatan pembuluh." },
  { "en": "Embolisme?", "id": "Penyumbatan." },
  { "en": "Embosur?", "id": "Cetakan timbul." },
  { "en": "Embrat?", "id": "Ember besar." },
  { "en": "Embrio?", "id": "Janin awal." },
  { "en": "Embriogenesis?", "id": "Pembentukan embrio." },
  { "en": "Embriologi?", "id": "Ilmu embrio." },
  { "en": "Embrionik?", "id": "Berkaitan embrio." },
  { "en": "Embuh?", "id": "Entah (Jawa)." },
  { "en": "Embuk?", "id": "Lembut." },
  { "en": "Embun?", "id": "Titik air pagi." },
  { "en": "Embung?", "id": "Waduk kecil." },
  { "en": "Embus?", "id": "Tiup." },
  { "en": "Emendasi?", "id": "Perbaikan naskah." },
  { "en": "Emeral?", "id": "Zamrud." },
  { "en": "Emergensi?", "id": "Darurat." },
  { "en": "Emetik?", "id": "Obat muntah." },
  { "en": "Emetina?", "id": "Senyawa obat." },
  { "en": "Emigran?", "id": "Pindah ke luar negeri." },
  { "en": "Emigrasi?", "id": "Kepindahan." },
  { "en": "Eminen?", "id": "Unggul." },
  { "en": "Eminensi?", "id": "Keunggulan." },
  { "en": "Emir?", "id": "Pemimpin Arab." },
  { "en": "Emirat?", "id": "Wilayah emir." },
  { "en": "Emis?", "id": "Kencing (arkais)." },
  { "en": "Emisi?", "id": "Pancaran." },
  { "en": "Emiten?", "id": "Penerbit saham." },
  { "en": "Emo?", "id": "Gaya hidup." },
  { "en": "Emodin?", "id": "Zat pencahar." },
  { "en": "Emong?", "id": "Asuh (Jawa)." },
  { "en": "Emosi?", "id": "Perasaan." },
  { "en": "Emosional?", "id": "Penuh perasaan." },
  { "en": "Empal?", "id": "Daging." },
  { "en": "Empan?", "id": "Cukup." },
  { "en": "Empang?", "id": "Kolam ikan." },
  { "en": "Empap?", "id": "Lembap (arkais)." },
  { "en": "Empas?", "id": "Hempas." },
  { "en": "Empat?", "id": "Angka." },
  { "en": "Empati?", "id": "Turut merasa." },
  { "en": "Empedal?", "id": "Ampela." },
  { "en": "Empedu?", "id": "Cairan pencernaan." },
  { "en": "Empelai?", "id": "Pengantin (arkais)." },
  { "en": "Empelam?", "id": "Mangga." },
  { "en": "Emper?", "id": "Teras." },
  { "en": "Empik?", "id": "Sakit (arkais)." },
  { "en": "Emping?", "id": "Keripik melinjo." },
  { "en": "Empiri?", "id": "Pengalaman." },
  { "en": "Empirik?", "id": "Berdasarkan pengalaman." },
  { "en": "Empiris?", "id": "Empirik." },
  { "en": "Empirisme?", "id": "Paham empirik." },
  { "en": "Emplak?", "id": "Makan (kasar)." },
  { "en": "Emplasemen?", "id": "Halaman stasiun." },
  { "en": "Empoh?", "id": "Banjir." },
  { "en": "Empok?", "id": "Kakak perempuan (Betawi)." },
  { "en": "Emporium?", "id": "Pusat dagang." },
  { "en": "Empos?", "id": "Sedot (arkais)." },
  { "en": "Empu?", "id": "Ahli pembuat keris." },
  { "en": "Empuk?", "id": "Lunak." },
  { "en": "Empul?", "id": "Kenyal." },
  { "en": "Empulur?", "id": "Isi batang." },
  { "en": "Emu?", "id": "Burung." },
  { "en": "Emulasi?", "id": "Peniruan." },
  { "en": "Emulator?", "id": "Peniru sistem." },
  { "en": "Emulsi?", "id": "Campuran cairan." },
  { "en": "Emulsifikasi?", "id": "Proses emulsi." },
  { "en": "Emut?", "id": "Isap." },
  { "en": "En?", "id": "Dan (Belanda)." },
  { "en": "Enak?", "id": "Lezat." },
  { "en": "Enam?", "id": "Angka." },
  { "en": "Enartrosis?", "id": "Sendi peluru." },
  { "en": "Enau?", "id": "Aren." },
  { "en": "Enca?", "id": "Cair (arkais)." },
  { "en": "Encang?", "id": "Cepat (arkais)." },
  { "en": "Encar?", "id": "Encer." },
  { "en": "Enceh?", "id": "Banyak (arkais)." },
  { "en": "Encek?", "id": "Paman (Tionghoa)." },
  { "en": "Encer?", "id": "Cair." },
  { "en": "Encik?", "id": "Panggilan wanita." },
  { "en": "Encim?", "id": "Bibi (Tionghoa)." },
  { "en": "Encing?", "id": "Bibi." },
  { "en": "Enco?", "id": "Lemah sendi." },
  { "en": "Encok?", "id": "Rematik." },
  { "en": "Encot?", "id": "Pincang." },
  { "en": "Enda?", "id": "Tidak (arkais)." },
  { "en": "Endak?", "id": "Tidak." },
  { "en": "Endal?", "id": "Kenyal." },
  { "en": "Endang?", "id": "Nama wanita." },
  { "en": "Endap?", "id": "Mengendap." },
  { "en": "Endasan?", "id": "Landasan (Jawa)." },
  { "en": "Endemi?", "id": "Penyakit khas wilayah." },
  { "en": "Endemis?", "id": "Bersifat endemi." },
  { "en": "Ender?", "id": "Turun (Belanda)." },
  { "en": "Endilau?", "id": "Pohon." },
  { "en": "Endoderm?", "id": "Lapisan dalam." },
  { "en": "Endoderma?", "id": "Endoderm." },
  { "en": "Endodermis?", "id": "Lapisan terdalam akar." },
  { "en": "Endofit?", "id": "Tumbuhan dalam tumbuhan lain." },
  { "en": "Endogami?", "id": "Kawin dalam suku." },
  { "en": "Endogen?", "id": "Berasal dari dalam." },
  { "en": "Endokrin?", "id": "Kelenjar hormon." },
  { "en": "Endokrinologi?", "id": "Ilmu hormon." },
  { "en": "Endometriosis?", "id": "Penyakit rahim." },
  { "en": "Endometrium?", "id": "Lapisan rahim." },
  { "en": "Endomisium?", "id": "Jaringan ikat otot." },
  { "en": "Endoparasit?", "id": "Parasit dalam tubuh." },
  { "en": "Endoplasma?", "id": "Bagian sel." },
  { "en": "Endorfin?", "id": "Hormon bahagia." },
  { "en": "Endosfer?", "id": "Bagian dalam bumi." },
  { "en": "Endoskeleton?", "id": "Rangka dalam." },
  { "en": "Endoskop?", "id": "Alat teropong tubuh." },
  { "en": "Endoskopi?", "id": "Peneropongan tubuh." },
  { "en": "Endosperma?", "id": "Cadangan makanan biji." },
  { "en": "Endotel?", "id": "Lapisan pembuluh." },
  { "en": "Endoterm?", "id": "Menyerap panas." },
  { "en": "Endotermis?", "id": "Bersifat endoterm." },
  { "en": "Endotoksin?", "id": "Racun bakteri." },
  { "en": "Endrin?", "id": "Insektisida." },
  { "en": "Enduk?", "id": "Panggilan sayang (Jawa)." },
  { "en": "Endul?", "id": "Ayunan." },
  { "en": "Enduro?", "id": "Balap motor." },
  { "en": "Endus?", "id": "Cium (bau)." },
  { "en": "Ene?", "id": "Nenek (Minang)." },
  { "en": "Enek?", "id": "Muak." },
  { "en": "Energi?", "id": "Tenaga." },
  { "en": "Enervasi?", "id": "Kelemahan." },
  { "en": "Enes?", "id": "Jijik (arkais)." },
  { "en": "Engah?", "id": "Tersengal." },
  { "en": "Engak?", "id": "Tidak (arkais)." },
  { "en": "Engan?", "id": "Ogah." },
  { "en": "Engap-engap?", "id": "Sulit bernapas." },
  { "en": "Engas?", "id": "Napas." },
  { "en": "Enggak?", "id": "Tidak (prokem)." },
  { "en": "Enggan?", "id": "Tidak mau." },
  { "en": "Engget?", "id": "Kait (arkais)." },
  { "en": "Enggil?", "id": "Gigit (arkais)." },
  { "en": "Engkah?", "id": "Buka (arkais)." },
  { "en": "Engkau?", "id": "Kamu." },
  { "en": "Engkel?", "id": "Pergelangan kaki." },
  { "en": "Engko?", "id": "Paman (Tionghoa)." },
  { "en": "Engkol?", "id": "Tuas putar." },
  { "en": "Engkong?", "id": "Kakek (Betawi)." },
  { "en": "Engku?", "id": "Panggilan hormat." },
  { "en": "Engkuk?", "id": "Burung." },
  { "en": "Engsel?", "id": "Sendi pintu." },
  { "en": "Enigma?", "id": "Teka-teki." },
  { "en": "Enigmatis?", "id": "Misterius." },
  { "en": "Enjak?", "id": "Injak." },
  { "en": "Enjal?", "id": "Sangga." },
  { "en": "Enjelai?", "id": "Jelai." },
  { "en": "Enjin?", "id": "Mesin." },
  { "en": "Enjiner?", "id": "Insinyur." },
  { "en": "Enjut?", "id": "Sentak." },
  { "en": "Enkapsulasi?", "id": "Pembungkusan." },
  { "en": "Enklave?", "id": "Wilayah kantong." },
  { "en": "Enkripsi?", "id": "Penyandian." },
  { "en": "Enkulturasi?", "id": "Pembudayaan." },
  { "en": "Enologi?", "id": "Ilmu anggur." },
  { "en": "Enom?", "id": "Muda (Jawa)." },
  { "en": "Ensambel?", "id": "Kelompok musik." },
  { "en": "Ensefalitis?", "id": "Radang otak." },
  { "en": "Ensefalograf?", "id": "Alat rekam otak." },
  { "en": "Ensefalogram?", "id": "Hasil rekam otak." },
  { "en": "Ensefalomielitis?", "id": "Radang otak sumsum." },
  { "en": "Ensefalon?", "id": "Otak." },
  { "en": "Enso?", "id": "Lingkaran Zen." },
  { "en": "Entah?", "id": "Tidak tahu." },
  { "en": "Entak?", "id": "Hentak." },
  { "en": "Entar?", "id": "Nanti (prokem)." },
  { "en": "Entas?", "id": "Angkat (dari air)." },
  { "en": "Ente?", "id": "Kamu (Arab)." },
  { "en": "Enten?", "id": "Bibit tanaman." },
  { "en": "Enteng?", "id": "Ringan." },
  { "en": "Enteritis?", "id": "Radang usus." },
  { "en": "Enterologi?", "id": "Ilmu usus." },
  { "en": "Enteron?", "id": "Rongga usus." },
  { "en": "Enteropati?", "id": "Penyakit usus." },
  { "en": "Enterostoksin?", "id": "Racun usus." },
  { "en": "Entit?", "id": "Ambil (arkais)." },
  { "en": "Entitas?", "id": "Wujud." },
  { "en": "Entok?", "id": "Itik manila." },
  { "en": "Entomofil?", "id": "Penyerbukan serangga." },
  { "en": "Entomolog?", "id": "Ahli serangga." },
  { "en": "Entomologi?", "id": "Ilmu serangga." },
  { "en": "Entong?", "id": "Panggilan anak laki-laki." },
  { "en": "Entot?", "id": "Sanggama (kasar)." },
  { "en": "Entradas?", "id": "Jalan masuk." },
  { "en": "Entri?", "id": "Kata masukan." },
  { "en": "Entropi?", "id": "Ukuran ketidakteraturan." },
  { "en": "Envoi?", "id": "Utusan diplomatik." },
  { "en": "Enzim?", "id": "Biokatalisator." },
  { "en": "Enzimologi?", "id": "Ilmu enzim." },
  { "en": "Eolit?", "id": "Batu zaman purba." },
  { "en": "Eon?", "id": "Masa sangat panjang." },
  { "en": "Eosen?", "id": "Kala geologi." },
  { "en": "Eosin?", "id": "Zat pewarna." },
  { "en": "Epa?", "id": "Satuan ukuran (Ibrani)." },
  { "en": "Epek?", "id": "Ikat pinggang." },
  { "en": "Ephedra?", "id": "Tanaman obat." },
  { "en": "Efemeral?", "id": "Singkat." },
  { "en": "Efemeris?", "id": "Tabel astronomi." },
  { "en": "Efendi?", "id": "Tuan (arkais)." },
  { "en": "Efialtes?", "id": "Mimpi buruk (Yunani)." },
  { "en": "Efisien?", "id": "Tepat guna." },
  { "en": "Efigrafi?", "id": "Ilmu prasasti." },
  { "en": "Efilasi?", "id": "Pawai." },
  { "en": "Epifisis?", "id": "Ujung tulang." },
  { "en": "Epifit?", "id": "Tumbuhan menumpang." },
  { "en": "Epifora?", "id": "Pengulangan kata akhir." },
  { "en": "Epigastrium?", "id": "Ulu hati." },
  { "en": "Epigenesis?", "id": "Perkembangan bertahap." },
  { "en": "Epiglotis?", "id": "Katup tenggorok." },
  { "en": "Epigon?", "id": "Peniru." },
  { "en": "Epigram?", "id": "Pernyataan singkat." },
  { "en": "Epigraf?", "id": "Prasasti." },
  { "en": "Epik?", "id": "Wiracarita." },
  { "en": "Epikotil?", "id": "Bagian kecambah." },
  { "en": "Epikuros?", "id": "Filsuf Yunani." },
  { "en": "Epidemi?", "id": "Wabah." },
  { "en": "Epidemiologi?", "id": "Ilmu wabah." },
  { "en": "Epidermis?", "id": "Kulit ari." },
  { "en": "Epidural?", "id": "Anestesi." },
  { "en": "Epifauna?", "id": "Hewan permukaan." },
  { "en": "Epifit?", "id": "Tanaman menumpang." },
  { "en": "Epigon?", "id": "Pengikut." },
  { "en": "Epilepsi?", "id": "Ayan." },
  { "en": "Epilog?", "id": "Penutup cerita." },
  { "en": "Epimisium?", "id": "Pembungkus otot." },
  { "en": "Epinefrina?", "id": "Adrenalin." },
  { "en": "Episentrum?", "id": "Pusat gempa." },
  { "en": "Episkopal?", "id": "Keuskupan." },
  { "en": "Episode?", "id": "Bagian cerita." },
  { "en": "Episodik?", "id": "Terjadi sesekali." },
  { "en": "Epistaksis?", "id": "Mimisan." },
  { "en": "Epistel?", "id": "Surat (resmi)." },
  { "en": "Epistemologi?", "id": "Filsafat pengetahuan." },
  { "en": "Epitaf?", "id": "Tulisan nisan." },
  { "en": "Epitalamus?", "id": "Bagian otak." },
  { "en": "Epitel?", "id": "Jaringan pelapis." },
  { "en": "Epitet?", "id": "Julukan." },
  { "en": "Epizoik?", "id": "Menempel pada hewan." },
  { "en": "Epizootik?", "id": "Wabah hewan." },
  { "en": "Epok?", "id": "Zaman." },
  { "en": "Epolet?", "id": "Tanda pangkat bahu." },
  { "en": "Eponim?", "id": "Nama dari orang." },
  { "en": "Epos?", "id": "Wiracarita." },
  { "en": "Epsilon?", "id": "Huruf Yunani." },
  { "en": "Era?", "id": "Zaman." },
  { "en": "Eradikasi?", "id": "Pemberantasan." },
  { "en": "Eram?", "id": "Mengeram." },
  { "en": "Erang?", "id": "Keluh." },
  { "en": "Erat?", "id": "Kuat (ikatan)." },
  { "en": "Erbium?", "id": "Unsur kimia." },
  { "en": "Ercis?", "id": "Kacang." },
  { "en": "Ere?", "id": "Kamu (arkais)." },
  { "en": "Ereksi?", "id": "Tegang (penis)." },
  { "en": "Eremologi?", "id": "Ilmu gurun." },
  { "en": "Erepsin?", "id": "Enzim." },
  { "en": "Eretan?", "id": "Tempat menyeberang." },
  { "en": "Erg?", "id": "Satuan energi." },
  { "en": "Ergonomika?", "id": "Ilmu kenyamanan kerja." },
  { "en": "Ergonomis?", "id": "Nyaman digunakan." },
  { "en": "Ergosterol?", "id": "Sterol." },
  { "en": "Ergot?", "id": "Jamur gandum." },
  { "en": "Ergoterap?", "id": "Terapi kerja." },
  { "en": "Erik?", "id": "Kantung (arkais)." },
  { "en": "Erip?", "id": "Tidur (Jawa)." },
  { "en": "Erisipelas?", "id": "Penyakit kulit." },
  { "en": "Eritema?", "id": "Kemerahan kulit." },
  { "en": "Eritrina?", "id": "Pohon dadap." },
  { "en": "Eritrosit?", "id": "Sel darah merah." },
  { "en": "Ermin?", "id": "Musang." },
  { "en": "Erosi?", "id": "Pengikisan." },
  { "en": "Erot?", "id": "Penyimpangan seksual." },
  { "en": "Erotika?", "id": "Seni birahi." },
  { "en": "Erotis?", "id": "Membangkitkan birahi." },
  { "en": "Erotisisme?", "id": "Hal erotis." },
  { "en": "Erotomania?", "id": "Kegilaan cinta." },
  { "en": "Erpak?", "id": "Hak sewa (Belanda)." },
  { "en": "Error?", "id": "Kesalahan (Inggris)." },
  { "en": "Erti?", "id": "Arti (ejaan lama)." },
  { "en": "Eru?", "id": "Pohon cemara." },
  { "en": "Erupsi?", "id": "Letusan." },
  { "en": "Es?", "id": "Air beku." },
  { "en": "Esa?", "id": "Tunggal." },
  { "en": "Esai?", "id": "Karangan." },
  { "en": "Esais?", "id": "Penulis esai." },
  { "en": "Esak?", "id": "Isak (arkais)." },
  { "en": "Esalon?", "id": "Tingkat kepangkatan." },
  { "en": "Esens?", "id": "Sari pati." },
  { "en": "Esensi?", "id": "Hakikat." },
  { "en": "Esensial?", "id": "Mendasar." },
  { "en": "Eskader?", "id": "Satuan kapal." },
  { "en": "Eskadron?", "id": "Satuan udara." },
  { "en": "Eskalasi?", "id": "Peningkatan." },
  { "en": "Eskalator?", "id": "Tangga jalan." },
  { "en": "Eskapisme?", "id": "Pelarian diri." },
  { "en": "Eskas?", "id": "Lemari es." },
  { "en": "Eskatologi?", "id": "Ajaran akhir zaman." },
  { "en": "Eskro?", "id": "Surat perjanjian." },
  { "en": "Eskudo?", "id": "Mata uang Portugal (lama)." },
  { "en": "Esok?", "id": "Besok." },
  { "en": "Eson?", "id": "Cucu (arkais)." },
  { "en": "Esoteris?", "id": "Khusus." },
  { "en": "Espas?", "id": "Jarak (arkais)." },
  { "en": "Esperanto?", "id": "Bahasa buatan." },
  { "en": "Estafet?", "id": "Lari sambung." },
  { "en": "Estampa?", "id": "Cetakan (Spanyol)." },
  { "en": "Estetik?", "id": "Indah." },
  { "en": "Estetika?", "id": "Ilmu keindahan." },
  { "en": "Estimasi?", "id": "Perkiraan." },
  { "en": "Estriol?", "id": "Hormon." },
  { "en": "Estrogen?", "id": "Hormon wanita." },
  { "en": "Estuari?", "id": "Muara sungai." },
  { "en": "Esuh?", "id": "Isap (arkais)." },
  { "en": "Eta?", "id": "Huruf Yunani." },
  { "en": "Etalase?", "id": "Lemari pajang." },
  { "en": "Etana?", "id": "Senyawa hidrokarbon." },
  { "en": "Etanol?", "id": "Alkohol." },
  { "en": "Etap?", "id": "Tahap." },
  { "en": "Etape?", "id": "Bagian perjalanan." },
  { "en": "Eter?", "id": "Zat pembius." },
  { "en": "Eternal?", "id": "Abadi (Inggris)." },
  { "en": "Etesan?", "id": "Saringan." },
  { "en": "Etnik?", "id": "Suku bangsa." },
  { "en": "Etnis?", "id": "Etnik." },
  { "en": "Etnobotani?", "id": "Ilmu tumbuhan etnik." },
  { "en": "Etnografi?", "id": "Deskripsi suku bangsa." },
  { "en": "Etnohistori?", "id": "Sejarah etnik." },
  { "en": "Etnolog?", "id": "Ahli etnologi." },
  { "en": "Etnologi?", "id": "Ilmu bangsa-bangsa." },
  { "en": "Etnomusikologi?", "id": "Ilmu musik etnik." },
  { "en": "Etnosentrisme?", "id": "Paham keunggulan suku." },
  { "en": "Eti?", "id": "Alat tenun." },
  { "en": "Etika?", "id": "Moral." },
  { "en": "Etiket?", "id": "Sopan santun." },
  { "en": "Etil?", "id": "Gugus kimia." },
  { "en": "Etimologi?", "id": "Ilmu asal kata." },
  { "en": "Etimon?", "id": "Kata asal." },
  { "en": "Etiolasi?", "id": "Pemucatan tanaman." },
  { "en": "Etiologi?", "id": "Ilmu sebab penyakit." },
  { "en": "Etis?", "id": "Sesuai etika." },
  { "en": "Eton?", "id": "Sekolah (Inggris)." },
  { "en": "Etsa?", "id": "Ukiran logam." },
  { "en": "Eufemisme?", "id": "Ungkapan halus." },
  { "en": "Eufoni?", "id": "Bunyi merdu." },
  { "en": "Euforia?", "id": "Rasa gembira." },
  { "en": "Eufrat?", "id": "Sungai." },
  { "en": "Eugenol?", "id": "Minyak cengkih." },
  { "en": "Euglena?", "id": "Protozoa." },
  { "en": "Eukaliptus?", "id": "Pohon kayu putih." },
  { "en": "Eukarion?", "id": "Sel kompleks." },
  { "en": "Ekses?", "id": "Kelebihan." },
  { "en": "Erosi?", "id": "Pengikisan." },
  { "en": "Erotisme?", "id": "Sifat erotis." },
  { "en": "Faal?", "id": "Fungsi." },
  { "en": "Fabel?", "id": "Cerita hewan." },
  { "en": "Fabliau?", "id": "Cerita jenaka (Prancis)." },
  { "en": "Fabrik?", "id": "Pabrik." },
  { "en": "Fabrikasi?", "id": "Pembuatan." },
  { "en": "Faset?", "id": "Permukaan." },
  { "en": "Faedah?", "id": "Manfaat." },
  { "en": "Fagosit?", "id": "Sel pemakan." },
  { "en": "Fagositosis?", "id": "Proses fagosit." },
  { "en": "Fahrenheit?", "id": "Skala suhu." },
  { "en": "Fail?", "id": "Gagal (Inggris)." },
  { "en": "Fajar?", "id": "Pagi buta." },
  { "en": "Fakih?", "id": "Ahli hukum Islam." },
  { "en": "Fakir?", "id": "Miskin." },
  { "en": "Faksimile?", "id": "Salinan persis." },
  { "en": "Faksi?", "id": "Golongan." },
  { "en": "Fakta?", "id": "Kenyataan." },
  { "en": "Faktual?", "id": "Berdasarkan fakta." },
  { "en": "Faktor?", "id": "Unsur." },
  { "en": "Faktur?", "id": "Tagihan." },
  { "en": "Fakultas?", "id": "Bagian universitas." },
  { "en": "Falaj?", "id": "Sistem irigasi (Arab)." },
  { "en": "Falak?", "id": "Ilmu bintang." },
  { "en": "Falsafah?", "id": "Filsafat." },
  { "en": "Falsafi?", "id": "Bersifat filsafat." },
  { "en": "Fam?", "id": "Marga (Tionghoa)." },
  { "en": "Fama?", "id": "Kabar (arkais)." },
  { "en": "Familia?", "id": "Keluarga." },
  { "en": "Familier?", "id": "Akrab." },
  { "en": "Fan?", "id": "Penggemar (Inggris)." },
  { "en": "Fana?", "id": "Tidak kekal." },
  { "en": "Fanatik?", "id": "Berlebihan." },
  { "en": "Fanatisme?", "id": "Sikap fanatik." },
  { "en": "Fani?", "id": "Fana." },
  { "en": "Fantasi?", "id": "Khayalan." },
  { "en": "Fantastis?", "id": "Luar biasa." },
  { "en": "Faraid?", "id": "Hukum waris Islam." },
  { "en": "Farak?", "id": "Beda (Arab)." },
  { "en": "Fardu?", "id": "Wajib." },
  { "en": "Farik?", "id": "Berbeda (Arab)." },
  { "en": "Farisi?", "id": "Orang Persia." },
  { "en": "Farmakolog?", "id": "Ahli obat." },
  { "en": "Farmakologi?", "id": "Ilmu obat." },
  { "en": "Farmakope?", "id": "Buku standar obat." },
  { "en": "Farmasi?", "id": "Ilmu perobatan." },
  { "en": "Fasakh?", "id": "Pembatalan nikah." },
  { "en": "Fascia?", "id": "Jaringan ikat." },
  { "en": "Fasi?", "id": "Fasih (arkais)." },
  { "en": "Fasid?", "id": "Rusak (Arab)." },
  { "en": "Fasih?", "id": "Lancar bicara." },
  { "en": "Fasik?", "id": "Berbuat dosa." },
  { "en": "Fasilitas?", "id": "Sarana." },
  { "en": "Fasis?", "id": "Penganut fasisme." },
  { "en": "Fasisme?", "id": "Paham politik." },
  { "en": "Fase?", "id": "Tahap." },
  { "en": "Faset?", "id": "Sisi (permata)." },
  { "en": "Fasiq?", "id": "Fasik." },
  { "en": "Fatah?", "id": "Kemenangan (Arab)." },
  { "en": "Fatal?", "id": "Berakibat buruk." },
  { "en": "Fatalis?", "id": "Penganut takdir." },
  { "en": "Fatalisme?", "id": "Paham takdir." },
  { "en": "Fatalitas?", "id": "Angka kematian." },
  { "en": "Fatanah?", "id": "Cerdas (sifat nabi)." },
  { "en": "Fatihah?", "id": "Surat Al-Fatihah." },
  { "en": "Fatir?", "id": "Tidak beragi (roti)." },
  { "en": "Fatom?", "id": "Ukuran kedalaman laut." },
  { "en": "Fatsun?", "id": "Sopan santun (Tionghoa)." },
  { "en": "Fatwa?", "id": "Nasihat hukum agama." },
  { "en": "Fauna?", "id": "Dunia hewan." },
  { "en": "Fauni?", "id": "Berkaitan fauna." },
  { "en": "Favorit?", "id": "Kesukaan." },
  { "en": "Favoritisme?", "id": "Pilih kasih." },
  { "en": "Faxes?", "id": "Faksimile (Inggris)." },
  { "en": "Februari?", "id": "Bulan kedua." },
  { "en": "Federal?", "id": "Serikat." },
  { "en": "Federalis?", "id": "Pendukung federalisme." },
  { "en": "Federalisme?", "id": "Paham negara serikat." },
  { "en": "Federasi?", "id": "Gabungan." },
  { "en": "Fehling?", "id": "Larutan kimia." },
  { "en": "Feko?", "id": "Kotoran manusia (arkais)." },
  { "en": "Felon?", "id": "Bisul jari." },
  { "en": "Felsik?", "id": "Batuan." },
  { "en": "Felspar?", "id": "Mineral." },
  { "en": "Feminin?", "id": "Kewanitaan." },
  { "en": "Feminisme?", "id": "Paham kesetaraan gender." },
  { "en": "Femto?", "id": "Awalan (10^-15)." },
  { "en": "Femur?", "id": "Tulang paha." },
  { "en": "Fenakit?", "id": "Mineral." },
  { "en": "Fenol?", "id": "Senyawa kimia." },
  { "en": "Fenologi?", "id": "Ilmu musim." },
  { "en": "Fenomena?", "id": "Gejala." },
  { "en": "Fenomenal?", "id": "Luar biasa." },
  { "en": "Fenotipe?", "id": "Sifat tampak." },
  { "en": "Feodal?", "id": "Bertuan tanah." },
  { "en": "Feodalisme?", "id": "Sistem feodal." },
  { "en": "Feri?", "id": "Kapal penyeberangan." },
  { "en": "Feritin?", "id": "Protein." },
  { "en": "Fermentasi?", "id": "Peragian." },
  { "en": "Fermion?", "id": "Partikel." },
  { "en": "Fermium?", "id": "Unsur kimia." },
  { "en": "Feron?", "id": "Aloi besi." },
  { "en": "Fertil?", "id": "Subur." },
  { "en": "Fertilisasi?", "id": "Pembuahan." },
  { "en": "Fertilitas?", "id": "Kesuburan." },
  { "en": "Feses?", "id": "Tinja." },
  { "en": "Festival?", "id": "Perayaan." },
  { "en": "Fetis?", "id": "Benda pujaan." },
  { "en": "Fetor?", "id": "Bau busuk." },
  { "en": "Fetus?", "id": "Janin." },
  { "en": "Feud?", "id": "Perseteruan (Inggris)." },
  { "en": "Fiat?", "id": "Persetujuan." },
  { "en": "Fiber?", "id": "Serat (Inggris)." },
  { "en": "Fibrasi?", "id": "Getaran." },
  { "en": "Fibril?", "id": "Serabut halus." },
  { "en": "Fibrilasi?", "id": "Getaran jantung." },
  { "en": "Fibrin?", "id": "Protein darah." },
  { "en": "Fibrinogen?", "id": "Protein plasma." },
  { "en": "Fibroblas?", "id": "Sel jaringan ikat." },
  { "en": "Fibroma?", "id": "Tumor jinak." },
  { "en": "Fibrosis?", "id": "Pembentukan jaringan parut." },
  { "en": "Fibula?", "id": "Tulang betis." },
  { "en": "Ficus?", "id": "Pohon ara." },
  { "en": "Fider?", "id": "Saluran (listrik)." },
  { "en": "Fidyah?", "id": "Tebusan puasa." },
  { "en": "Figur?", "id": "Tokoh." },
  { "en": "Figuran?", "id": "Pemeran pembantu." },
  { "en": "Figuratif?", "id": "Kiasan." },
  { "en": "Fikih?", "id": "Hukum Islam." },
  { "en": "Fikir?", "id": "Pikir (ejaan lama)." },
  { "en": "Fiksasi?", "id": "Penetapan." },
  { "en": "Fiksi?", "id": "Rekaan." },
  { "en": "Fiktif?", "id": "Tidak nyata." },
  { "en": "Fikus?", "id": "Ficus." },
  { "en": "Filamen?", "id": "Benang halus." },
  { "en": "Filantrop?", "id": "Dermawan." },
  { "en": "Filantropi?", "id": "Kedermawanan." },
  { "en": "Filariasis?", "id": "Penyakit kaki gajah." },
  { "en": "Filateli?", "id": "Hobi prangko." },
  { "en": "Filatelis?", "id": "Pengumpul prangko." },
  { "en": "File?", "id": "Berkas (Inggris)." },
  { "en": "Filibuster?", "id": "Penghambatan sidang." },
  { "en": "Film?", "id": "Gambar hidup." },
  { "en": "Filolog?", "id": "Ahli filologi." },
  { "en": "Filologi?", "id": "Ilmu naskah kuno." },
  { "en": "Filosof?", "id": "Filsuf." },
  { "en": "Filosofi?", "id": "Filsafat." },
  { "en": "Filosofis?", "id": "Bersifat filsafat." },
  { "en": "Filter?", "id": "Saringan." },
  { "en": "Filum?", "id": "Tingkatan taksonomi." },
  { "en": "Final?", "id": "Akhir." },
  { "en": "Finale?", "id": "Bagian akhir." },
  { "en": "Finansial?", "id": "Keuangan." },
  { "en": "Finir?", "id": "Lapisan kayu." },
  { "en": "Finis?", "id": "Garis akhir." },
  { "en": "Fira?", "id": "Jalan (arkais)." },
  { "en": "Firaj?", "id": "Kelegaan (Arab)." },
  { "en": "Firasat?", "id": "Perasaan akan terjadi." },
  { "en": "Firdaus?", "id": "Surga." },
  { "en": "Firjat?", "id": "Celah (arkais)." },
  { "en": "Firma?", "id": "Perusahaan." },
  { "en": "Firman?", "id": "Perkataan Tuhan." },
  { "en": "Fisi?", "id": "Pembelahan." },
  { "en": "Fisibel?", "id": "Dapat dilakukan." },
  { "en": "Fisibilitas?", "id": "Kelayakan." },
  { "en": "Fisik?", "id": "Jasmani." },
  { "en": "Fisika?", "id": "Ilmu alam." },
  { "en": "Fisiognomi?", "id": "Ilmu firasat wajah." },
  { "en": "Fisiognomis?", "id": "Berkaitan wajah." },
  { "en": "Fisiokrat?", "id": "Penganut fisiokrasi." },
  { "en": "Fisiokrasi?", "id": "Paham ekonomi." },
  { "en": "Fisiolog?", "id": "Ahli fisiologi." },
  { "en": "Fisiologi?", "id": "Ilmu fungsi tubuh." },
  { "en": "Fisioterapi?", "id": "Terapi fisik." },
  { "en": "Fiting?", "id": "Penyambung pipa." },
  { "en": "Fitnah?", "id": "Tuduhan palsu." },
  { "en": "Fitometer?", "id": "Pengukur pertumbuhan." },
  { "en": "Fitoplankton?", "id": "Plankton tumbuhan." },
  { "en": "Fitrah?", "id": "Sifat asal." },
  { "en": "Fitri?", "id": "Suci." },
  { "en": "Flakon?", "id": "Botol kecil." },
  { "en": "Flamboyan?", "id": "Pohon." },
  { "en": "Flamingo?", "id": "Burung." },
  { "en": "Flanel?", "id": "Kain." },
  { "en": "Flat?", "id": "Apartemen." },
  { "en": "Flegma?", "id": "Dahak." },
  { "en": "Flegmatik?", "id": "Tenang." },
  { "en": "Flek?", "id": "Noda." },
  { "en": "Fleksi?", "id": "Pembengkokan." },
  { "en": "Fleksibel?", "id": "Luwes." },
  { "en": "Fleksibilitas?", "id": "Keluwesan." },
  { "en": "Flensa?", "id": "Sambungan pipa." },
  { "en": "Flirer?", "id": "Menggoda (Inggris)." },
  { "en": "Flis?", "id": "Kain tebal." },
  { "en": "Floem?", "id": "Jaringan tumbuhan." },
  { "en": "Flora?", "id": "Dunia tumbuhan." },
  { "en": "Floret?", "id": "Bunga kecil." },
  { "en": "Flotasi?", "id": "Pengapungan." },
  { "en": "Flotila?", "id": "Armada kecil." },
  { "en": "Flu?", "id": "Influenza." },
  { "en": "Fluks?", "id": "Aliran." },
  { "en": "Fluktuasi?", "id": "Perubahan." },
  { "en": "Fluktuatif?", "id": "Berubah-ubah." },
  { "en": "Fluoresen?", "id": "Berpendar." },
  { "en": "Fluorida?", "id": "Senyawa fluor." },
  { "en": "Fluorin?", "id": "Unsur kimia." },
  { "en": "Fluorit?", "id": "Mineral." },
  { "en": "Fluviatil?", "id": "Sungai." },
  { "en": "Fokus?", "id": "Pusat." },
  { "en": "Folder?", "id": "Map (Inggris)." },
  { "en": "Foli?", "id": "Daun (arkais)." },
  { "en": "Folikel?", "id": "Kantung kecil." },
  { "en": "Folio?", "id": "Ukuran kertas." },
  { "en": "Folklor?", "id": "Cerita rakyat." },
  { "en": "Fon?", "id": "Bunyi bahasa." },
  { "en": "Fonasi?", "id": "Pengucapan bunyi." },
  { "en": "Fondamen?", "id": "Dasar." },
  { "en": "Fondasi?", "id": "Pondasi." },
  { "en": "Fonem?", "id": "Satuan bunyi." },
  { "en": "Fonemik?", "id": "Berkaitan fonem." },
  { "en": "Fonetik?", "id": "Ilmu bunyi bahasa." },
  { "en": "Fonis?", "id": "Berkaitan bunyi." },
  { "en": "Fonograf?", "id": "Perekam suara." },
  { "en": "Fonografi?", "id": "Penulisan bunyi." },
  { "en": "Fonologi?", "id": "Ilmu sistem bunyi." },
  { "en": "Fonon?", "id": "Kuantum getaran." },
  { "en": "Font?", "id": "Jenis huruf." },
  { "en": "Fontanel?", "id": "Ubun-ubun." },
  { "en": "Fora?", "id": "Jamak dari forum." },
  { "en": "Forensik?", "id": "Ilmu bukti hukum." },
  { "en": "Forklif?", "id": "Alat angkut." },
  { "en": "Forma?", "id": "Bentuk." },
  { "en": "Formal?", "id": "Resmi." },
  { "en": "Formaldehida?", "id": "Formalin." },
  { "en": "Formalin?", "id": "Pengawet." },
  { "en": "Formalitas?", "id": "Tata cara." },
  { "en": "Formasi?", "id": "Susunan." },
  { "en": "Format?", "id": "Bentuk." },
  { "en": "Formatif?", "id": "Membentuk." },
  { "en": "Formatur?", "id": "Pembentuk." },
  { "en": "Formika?", "id": "Pelapis." },
  { "en": "Formula?", "id": "Rumus." },
  { "en": "Formulasi?", "id": "Perumusan." },
  { "en": "Formulir?", "id": "Isian." },
  { "en": "Fornifikasi?", "id": "Perzinaan." },
  { "en": "Forsir?", "id": "Paksa." },
  { "en": "Fortifikasi?", "id": "Perbentengan." },
  { "en": "Forum?", "id": "Wadah diskusi." },
  { "en": "Fosfat?", "id": "Garam asam fosfor." },
  { "en": "Fosfina?", "id": "Gas beracun." },
  { "en": "Fosfor?", "id": "Unsur kimia." },
  { "en": "Fosforesens?", "id": "Pendaran." },
  { "en": "Fosil?", "id": "Sisa purba." },
  { "en": "Foto?", "id": "Gambar." },
  { "en": "Fotodiode?", "id": "Komponen elektronik." },
  { "en": "Fotogenik?", "id": "Bagus difoto." },
  { "en": "Fotografer?", "id": "Juru foto." },
  { "en": "Fotografi?", "id": "Seni foto." },
  { "en": "Fotokopi?", "id": "Salinan." },
  { "en": "Fotolisis?", "id": "Penguraian cahaya." }


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
            }, 4000);
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
