# rtl_433
本forkは、非公式に作成したrtl_433の日本語ドキュメント作成用のレポジトリです。誤訳、珍訳はご連絡ください。

（※訳注：docs/OPERATION.md が参考になります）

rtl_433は（名前に反して）汎用のRFプロトコルアナライザであり、主として433.92 MHz、868 MHz（SRD）、315 MHz、345 MHz、及び 915 MHz ISM バンドを目的としたものです。
公式のソースコードは、https://github.com/merbanan/rtl_433/ のレポジトリにあります。
資料及び関連のプロジェクトについては、 https://triq.org/ を参照してください。

rtl_433は、 [RTL-SDR](https://github.com/osmocom/rtl-sdr/) 及び [SoapySDR](https://github.com/pothosware/SoapySDR/)で動作します。
Realtek RTL2832 based DVB dongles (using RTL-SDR) 及び LimeSDR ([LimeSDR USB](https://www.crowdsupply.com/lime-micro/limesdr) 、 [LimeSDR mini](https://www.crowdsupply.com/lime-micro/limesdr-mini) [MyriadRf](https://myriadrf.org/) により、開発者向けのサンプルが配布されています), PlutoSDR, HackRF One (SoapySDR ドライバを使用している)、同様に SoapyRemote　のデバイスについて、開発とサポートが活発に行われております。

![rtl_433 screenshot](./docs/screenshot.png)

## ビルド及びインストール方法

rtl_433は、portable C(C99 スタンダード)で記述されており、Linux (also embedded)、MacOS、そしてWindows systemsでコンパイル可能です。
過去のコンパイラやツールチェーンのサポートも重要な目標です。
低リソース消費及び低依存度により、rtl_433は（転用）ルーター等の組み込み機器で動作可能です。
32-bit i686 及び 64-bit x86-64 、同様に (組み込み) ARM、つまりRaspberry Pi や PlutoSDR に使用されているアーキテクチャはサポートの対象となっています。

詳細は右記参照のこと [BUILDING.md](docs/BUILDING.md)

Debian (sid) 又は Ubuntu (19.10+) 環境下では、 `apt-get install rtl-433` for other distros check https://repology.org/project/rtl-433/versions

FreeBSD では、 `pkg install rtl-433`。

On MacOS では、 `brew install rtl_433`。

rtl_433のDockerイメージは以下で有効です。 [on the github page of hertzg](https://github.com/hertzg/rtl_433_docker).

## 未サポートのセンサを追加する方法

[CONTRIBUTING.md](./docs/CONTRIBUTING.md).　を参照のこと。

## 実行方法

    rtl_433 -h

```

		= 一般オプション =
  [-V] バージョン情報を表示し、終了する。
  [-v] Increase verbosity (このオプションは複数回追加して使用できます).
       -v : verbose, -vv : verbose decoders, -vvv : debug decoders, -vvvv : trace decoding).
  [-c <path>] 設定オプションをファイルから読み込む。
		= Tuner オプション =
  [-d <RTL-SDR USB device index> | :<RTL-SDR USB デバイスシリアル> | <SoapySDR デバイスクエリ> | rtl_tcp | help]
  [-g <利得> | help] (デフォルト: auto)
  [-t <設定>] SoapySDR デバイスの keyword=value 設定リストを適用する。
       例： -t "antenna=A,bandwidth=4.5M,rfnotch_ctrl=false"
  [-f <周波数>] 受信周波数 (デフォルト: 433920000 Hz)
  [-H <秒数>] 複数の周波数を巡回するためのホッピング間隔 (デフォルト: 600 秒)
  [-p <ppm_error] rtl-sdr のチューナーに対する周波数オフセットエラーの校正値 (デフォルト: 0)
  [-s <sample rate>] サンプルレートの設定 (デフォルト: 250000 Hz)
		= 復調オプション =
  [-R <デバイス> | help] 特定のデバイスに対する復調プロトコルを有効にする (このオプションは複数回追加して使用できます)
       Specify a negative number to disable a device decoding protocol (このオプションは複数回追加して使用できます)
  [-G] Enable blacklisted device decoding protocols, for testing only.
  [-X <spec> | help] 汎用目的デコーダを追加する (prepend -R 0 to disable all decoders)
  [-Y auto | classic | minmax] FSK パルス検出モード
  [-Y level=<dB level>] Manual detection level used to determine pulses (-1.0 to -30.0) (0=auto).
  [-Y minlevel=<dB level>] Manual minimum detection level used to determine pulses (-1.0 to -99.0).
  [-Y minsnr=<dB level>] Minimum SNR to determine pulses (1.0 to 99.0).
  [-Y autolevel] Set minlevel automatically based on average estimated noise.
  [-Y squelch] Skip frames below estimated noise level to reduce cpu load.
  [-Y ampest | magest] Choose amplitude or magnitude level estimator.
		= 解析/デバッグ オプション =
  [-a] 解析モード。 受信信号の詳細をテキストで表示する。
  [-A] パルスアナライザ。パルス解析を可能とし、デコードを試みる。
       Disable all decoders with -R 0 if you want analyzer output only.
  [-y <code>] Verify decoding of demodulated test data (e.g. "{25}fb2dd58") with enabled devices
		= ファイル I/O オプション =
  [-S none | all | unknown | known] 信号の自動保存。 Creates one file per signal.
       Note: Saves raw I/Q samples (uint8 pcm, 2 channel). Preferred mode for generating test files.
  [-r <ファイル名> | ヘルプ] データを受信機ではなく入力ファイルから読み込む。
  [-w <ファイル名> | ヘルプ] データストリームを出力ファイルに保存する。 (a '-' dumps samples to stdout)
  [-W <ファイル名> | ヘルプ] データストリームを出力ファイルに保存し、既に存在するファイルに上書きする。
		= データ出力オプション =
  [-F kv | json | csv | mqtt | influx | syslog | null | help] Produce decoded output in given format.
       Append output to file with :<ファイル名> (例： -F csv:log.csv), defaults to stdout.
       Specify host/port for syslog with 例： -F syslog:127.0.0.1:1514
  [-M time[:<オプション>] | protocol | level | noise[:secs] | stats | bits | help] Add various meta data to each output.
  [-K FILE | PATH | <tag> | <key>=<tag>] Add an expanded token or fixed tag to every output line.
  [-C native | si | customary] Convert units in decoded output.
  [-n <値>] Specify number of samples to take (each sample is an I/Q pair)
  [-T <秒数>] 実行秒数を指定する。形式は 12:34 又は 1h23m45s の例による。
  [-E hop | quit] Hop/Quit after outputting successful event(s)
  [-h] この使用方法を表示し、終了する。
       -d, -g, -R, -X, -F, -M, -r, -w, 又は -W を引数なしで使用すると、詳細なヘルプを表示します。



		= サポート対象のデバイスプロトコル =
    [01]  Silvercrest Remote Control
    [02]  Rubicson Temperature Sensor
    [03]  Prologue, FreeTec NC-7104, NC-7159-675 temperature sensor
    [04]  Waveman Switch Transmitter
    [06]* ELV EM 1000
    [07]* ELV WS 2000
    [08]  LaCrosse TX Temperature / Humidity Sensor
    [10]* Acurite 896 Rain Gauge
    [11]  Acurite 609TXC Temperature and Humidity Sensor
    [12]  Oregon Scientific Weather Sensor
    [13]* Mebus 433
    [14]* Intertechno 433
    [15]  KlikAanKlikUit Wireless Switch
    [16]  AlectoV1 Weather Sensor (Alecto WS3500 WS4500 Ventus W155/W044 Oregon)
    [17]  Cardin S466-TX2
    [18]  Fine Offset Electronics, WH2, WH5, Telldus Temperature/Humidity/Rain Sensor
    [19]  Nexus, FreeTec NC-7345, NX-3980, Solight TE82S, TFA 30.3209 temperature/humidity sensor
    [20]  Ambient Weather, TFA 30.3208.02 temperature sensor
    [21]  Calibeur RF-104 Sensor
    [22]  X10 RF
    [23]  DSC Security Contact
    [24]* Brennenstuhl RCS 2044
    [25]  Globaltronics GT-WT-02 Sensor
    [26]  Danfoss CFR Thermostat
    [29]  Chuango Security Technology
    [30]  Generic Remote SC226x EV1527
    [31]  TFA-Twin-Plus-30.3049, Conrad KW9010, Ea2 BL999
    [32]  Fine Offset Electronics WH1080/WH3080 Weather Station
    [33]  WT450, WT260H, WT405H
    [34]  LaCrosse WS-2310 / WS-3600 Weather Station
    [35]  Esperanza EWS
    [36]  Efergy e2 classic
    [37]* Inovalley kw9015b, TFA Dostmann 30.3161 (Rain and temperature sensor)
    [38]  Generic temperature sensor 1
    [39]  WG-PB12V1 Temperature Sensor
    [40]  Acurite 592TXR Temp/Humidity, 5n1 Weather Station, 6045 Lightning, 3N1, Atlas
    [41]  Acurite 986 Refrigerator / Freezer Thermometer
    [42]  HIDEKI TS04 Temperature, Humidity, Wind and Rain Sensor
    [43]  Watchman Sonic / Apollo Ultrasonic / Beckett Rocket oil tank monitor
    [44]  CurrentCost Current Sensor
    [45]  emonTx OpenEnergyMonitor
    [46]  HT680 Remote control
    [47]  Conrad S3318P, FreeTec NC-5849-913 temperature humidity sensor
    [48]  Akhan 100F14 remote keyless entry
    [49]  Quhwa
    [50]  OSv1 Temperature Sensor
    [51]  Proove / Nexa / KlikAanKlikUit Wireless Switch
    [52]  Bresser Thermo-/Hygro-Sensor 3CH
    [53]  Springfield Temperature and Soil Moisture
    [54]  Oregon Scientific SL109H Remote Thermal Hygro Sensor
    [55]  Acurite 606TX Temperature Sensor
    [56]  TFA pool temperature sensor
    [57]  Kedsum Temperature & Humidity Sensor, Pearl NC-7415
    [58]  Blyss DC5-UK-WH
    [59]  Steelmate TPMS
    [60]  Schrader TPMS
    [61]* LightwaveRF
    [62]* Elro DB286A Doorbell
    [63]  Efergy Optical
    [64]* Honda Car Key
    [67]  Radiohead ASK
    [68]  Kerui PIR / Contact Sensor
    [69]  Fine Offset WH1050 Weather Station
    [70]  Honeywell Door/Window Sensor, 2Gig DW10/DW11, RE208 repeater
    [71]  Maverick ET-732/733 BBQ Sensor
    [72]* RF-tech
    [73]  LaCrosse TX141-Bv2, TX141TH-Bv2, TX141-Bv3, TX141W, TX145wsdth sensor
    [74]  Acurite 00275rm,00276rm Temp/Humidity with optional probe
    [75]  LaCrosse TX35DTH-IT, TFA Dostmann 30.3155 Temperature/Humidity sensor
    [76]  LaCrosse TX29IT, TFA Dostmann 30.3159.IT Temperature sensor
    [77]  Vaillant calorMatic VRT340f Central Heating Control
    [78]  Fine Offset Electronics, WH25, WH32B, WH24, WH65B, HP1000 Temperature/Humidity/Pressure Sensor
    [79]  Fine Offset Electronics, WH0530 Temperature/Rain Sensor
    [80]  IBIS beacon
    [81]  Oil Ultrasonic STANDARD FSK
    [82]  Citroen TPMS
    [83]  Oil Ultrasonic STANDARD ASK
    [84]  Thermopro TP11 Thermometer
    [85]  Solight TE44/TE66, EMOS E0107T, NX-6876-917
    [86]  Wireless Smoke and Heat Detector GS 558
    [87]  Generic wireless motion sensor
    [88]  Toyota TPMS
    [89]  Ford TPMS
    [90]  Renault TPMS
    [91]  inFactory, nor-tec, FreeTec NC-3982-913 temperature humidity sensor
    [92]  FT-004-B Temperature Sensor
    [93]  Ford Car Key
    [94]  Philips outdoor temperature sensor (type AJ3650)
    [95]  Schrader TPMS EG53MA4, PA66GF35
    [96]  Nexa
    [97]  Thermopro TP08/TP12/TP20 thermometer
    [98]  GE Color Effects
    [99]  X10 Security
    [100]  Interlogix GE UTC Security Devices
    [101]* Dish remote 6.3
    [102]  SimpliSafe Home Security System (May require disabling automatic gain for KeyPad decodes)
    [103]  Sensible Living Mini-Plant Moisture Sensor
    [104]  Wireless M-Bus, Mode C&T, 100kbps (-f 868950000 -s 1200000)
    [105]  Wireless M-Bus, Mode S, 32.768kbps (-f 868300000 -s 1000000)
    [106]* Wireless M-Bus, Mode R, 4.8kbps (-f 868330000)
    [107]* Wireless M-Bus, Mode F, 2.4kbps
    [108]  Hyundai WS SENZOR Remote Temperature Sensor
    [109]  WT0124 Pool Thermometer
    [110]  PMV-107J (Toyota) TPMS
    [111]  Emos TTX201 Temperature Sensor
    [112]  Ambient Weather TX-8300 Temperature/Humidity Sensor
    [113]  Ambient Weather WH31E Thermo-Hygrometer Sensor, EcoWitt WH40 rain gauge
    [114]  Maverick et73
    [115]  Honeywell ActivLink, Wireless Doorbell
    [116]  Honeywell ActivLink, Wireless Doorbell (FSK)
    [117]* ESA1000 / ESA2000 Energy Monitor
    [118]* Biltema rain gauge
    [119]  Bresser Weather Center 5-in-1
    [120]* Digitech XC-0324 temperature sensor
    [121]  Opus/Imagintronix XT300 Soil Moisture
    [122]* FS20
    [123]* Jansite TPMS Model TY02S
    [124]  LaCrosse/ELV/Conrad WS7000/WS2500 weather sensors
    [125]  TS-FT002 Wireless Ultrasonic Tank Liquid Level Meter With Temperature Sensor
    [126]  Companion WTR001 Temperature Sensor
    [127]  Ecowitt Wireless Outdoor Thermometer WH53/WH0280/WH0281A
    [128]  DirecTV RC66RX Remote Control
    [129]* Eurochron temperature and humidity sensor
    [130]  IKEA Sparsnas Energy Meter Monitor
    [131]  Microchip HCS200 KeeLoq Hopping Encoder based remotes
    [132]  TFA Dostmann 30.3196 T/H outdoor sensor
    [133]  Rubicson 48659 Thermometer
    [134]  Holman Industries iWeather WS5029 weather station (newer PCM)
    [135]  Philips outdoor temperature sensor (type AJ7010)
    [136]  ESIC EMT7110 power meter
    [137]  Globaltronics QUIGG GT-TMBBQ-05
    [138]  Globaltronics GT-WT-03 Sensor
    [139]  Norgo NGE101
    [140]  Elantra2012 TPMS
    [141]  Auriol HG02832, HG05124A-DCF, Rubicson 48957 temperature/humidity sensor
    [142]  Fine Offset Electronics/ECOWITT WH51 Soil Moisture Sensor
    [143]  Holman Industries iWeather WS5029 weather station (older PWM)
    [144]  TBH weather sensor
    [145]  WS2032 weather station
    [146]  Auriol AFW2A1 temperature/humidity sensor
    [147]  TFA Drop Rain Gauge 30.3233.01
    [148]  DSC Security Contact (WS4945)
    [149]  ERT Standard Consumption Message (SCM)
    [150]* Klimalogg
    [151]  Visonic powercode
    [152]  Eurochron EFTH-800 temperature and humidity sensor
    [153]  Cotech 36-7959 wireless weather station with USB
    [154]  Standard Consumption Message Plus (SCMplus)
    [155]  Fine Offset Electronics WH1080/WH3080 Weather Station (FSK)
    [156]  Abarth 124 Spider TPMS
    [157]  Missil ML0757 weather station
    [158]  Sharp SPC775 weather station
    [159]  Insteon
    [160]  ERT Interval Data Message (IDM)
    [161]  ERT Interval Data Message (IDM) for Net Meters
    [162]* ThermoPro-TX2 temperature sensor
    [163]  Acurite 590TX Temperature with optional Humidity
    [164]  Security+ 2.0 (Keyfob)
    [165]  TFA Dostmann 30.3221.02 T/H Outdoor Sensor
    [166]  LaCrosse Technology View LTV-WSDTH01 Breeze Pro Wind Sensor
    [167]  Somfy RTS
    [168]  Schrader TPMS SMD3MA4 (Subaru)
    [169]* Nice Flor-s remote control for gates
    [170]  LaCrosse Technology View LTV-WR1 Multi Sensor
    [171]  LaCrosse Technology View LTV-TH Thermo/Hygro Sensor
    [172]  Bresser Weather Center 6-in-1, 7-in-1 indoor, new 5-in-1, 3-in-1 wind gauge, Froggit WH6000, Ventus C8488A
    [173]  Bresser Weather Center 7-in-1
    [174]  EcoDHOME Smart Socket and MCEE Solar monitor
    [175]  LaCrosse Technology View LTV-R1 Rainfall Gauge
    [176]  BlueLine Power Monitor
    [177]  Burnhard BBQ thermometer
    [178]  Security+ (Keyfob)
    [179]  Cavius smoke, heat and water detector
    [180]  Jansite TPMS Model Solar
    [181]  Amazon Basics Meat Thermometer
    [182]  TFA Marbella Pool Thermometer
    [183]  Auriol AHFL temperature/humidity sensor
    [184]  Auriol AFT 77 B2 temperature sensor
    [185]  Honeywell CM921 Wireless Programmable Room Thermostat
    [186]  Hyundai TPMS (VDO)
    [187]  RojaFlex shutter and remote devices
    [188]  Marlec Solar iBoost+ sensors
    [189]  Somfy io-homecontrol
    [190]  Ambient Weather (Fine Offset) WH31L Lightning-Strike sensor

* デフォルトでは無効である。使用の際は -R n 又は -G とすること。


		= 入力デバイスの選択 =
	RTL-SDR のデバイスドライバは利用可能です。
  [-d <RTL-SDR USB デバイスインデックス>] (デフォルト: 0)
  [-d :<RTL-SDR USB デバイスシリアル (can be set with rtl_eeprom -s)>]
	To set gain for RTL-SDR use -g <利得> to set an overall gain in dB.
	SoapySDR デバイスドライバで使用可能である。
  [-d ""] デフォルトの SoapySDR デバイスを開く。
  [-d driver=rtlsdr] Open e.g. specific SoapySDR device
	To set gain for SoapySDR use -g ELEM=val,ELEM=val,... e.g. -g LNA=20,TIA=8,PGA=2 (LimeSDR 用).
  [-d rtl_tcp[:[//]host[:port]] (default: localhost:1234)
	Specify host/port to connect to with e.g. -d rtl_tcp:127.0.0.1:1234


		= 利得オプション =
  [-g <利得>] (デフォルト：自動)
	RTL-SDR: 利得を dB で指定 ("0" の場合、自動となる)。
	SoapySDR: 利得を dB で自動分配する ("" とすることで自動となる)か、 利得の各要素（訳注：LNA、VGA、AMP等、SDRデバイスによる）を文字列で指定する。
	例： "LNA=20,TIA=8,PGA=2" for LimeSDR.


		= Flex デコーダ spec =
-X <spec> オプションにより、汎用フレックスデコーダを使用可能にできる。
（訳注：パルス変調に関する各種定義は、https://www.keyence.co.jp/ss/products/recorder/lab/pulse/base.jspが詳しい）

<spec> is "key=value[,key=value...]"
共通の keys は以下の通り:
	name=<name> (or: n=<name>)
	modulation=<modulation> (or: m=<modulation>)
	short=<short> (or: s=<short>)
	long=<long> (or: l=<long>)
	sync=<sync> (or: y=<sync>)
	reset=<reset> (or: r=<reset>)
	gap=<gap> (or: g=<gap>)
	tolerance=<tolerance> (or: t=<tolerance>)
where:
<name> can be any descriptive name tag you need in the output
<変調方式> は下記のいずれかである:
	OOK_MC_ZEROBIT :  マンチェスター符号 with fixed leading zero bit
	OOK_PCM :         パルス符号変調 (RZ or NRZ)
	OOK_PPM :         パルス位置変調
	OOK_PWM :         パルス幅変調
	OOK_DMC :         差分マンチェスター符号
	OOK_PIWM_RAW :    Raw Pulse Interval and Width Modulation
	OOK_PIWM_DC :     差分パルス繰り返し変調及びパルス幅変調
	OOK_MC_OSV1 :     マンチェスター符号 for OSv1 devices
	FSK_PCM :         FSK パルス符号変調
	FSK_PWM :         FSK パルス幅変調
	FSK_MC_ZEROBIT :  Manchester Code with fixed leading zero bit
<short>, <long>, <sync> are nominal modulation timings in us,
<reset>, <gap>, <tolerance> are maximum modulation timings in us:
PCM     short: 公称パルス幅 [us]
         long: 公称ビット期間幅 [us]
PPM     short: Nominal width of '0' gap [us]
         long: Nominal width of '1' gap [us]
PWM     short: Nominal width of '1' pulse [us]
         long: Nominal width of '0' pulse [us]
         sync: Nominal width of sync pulse [us] (optional)
common    gap: 次のビット列が始まるまでの最大空白時間 [us]
        reset: Maximum gap size before End Of Message [us]
    tolerance: Maximum pulse deviation [us] (optional).
選択可能なオプションは下記の通り:
	bits=<n> : only match if at least one row has <n> bits
	rows=<n> : only match if there are <n> rows
	repeats=<n> : only match if some row is repeated <n> times
		use opt>=n to match at least <n> and opt<=n to match at most <n>
	invert : 全ての bit を反転する
	reflect : reflect each byte (MSB first to MSB last)
	match=<bits> : only match if the <bits> are found
	preamble=<bits> : match and align at the <bits> preamble
		<bits> is a row spec of {<bit count>}<bits as hex number>
	unique : suppress duplicate row output

	countonly : suppress detailed row output

例： -X "n=doorbell,m=OOK_PWM,s=400,l=800,r=7000,g=1000,match={24}0xa9878c,repeats>=3"



		= 出力フォーマットオプション =
  [-F kv|json|csv|mqtt|influx|syslog|null] 復調済み出力を指定フォーマットで生成する。
	このオプションを指定しなかった場合、デフォルトではKVが出力形式となる。 Use "-F null" to remove the default.
	Append output to file with :<ファイル名> (例： -F csv:log.csv), defaults to stdout.
	Specify MQTT server with 例： -F mqtt://localhost:1883
	Add MQTT options with 例： -F "mqtt://host:1883,opt=arg"
	MQTT options are: user=foo, pass=bar, retain[=0|1], <format>[=topic]
	Supported MQTT formats: (default is all)
	  events: posts JSON event data
	  states: posts JSON state data
	  devices: posts device and sensor info in nested topics
	The topic string will expand keys like [/model]
	例： -F "mqtt://localhost:1883,user=USERNAME,pass=PASSWORD,retain=0,devices=rtl_433[/id]"
	With MQTT each rtl_433 instance needs a distinct driver selection. The MQTT Client-ID is computed from the driver string.
	If you use multiple RTL-SDR, perhaps set a serial and select by that (helps not to get the wrong antenna).
	Specify InfluxDB 2.0 server with e.g. -F "influx://localhost:9999/api/v2/write?org=<org>&bucket=<bucket>,token=<authtoken>"
	Specify InfluxDB 1.x server with e.g. -F "influx://localhost:8086/write?db=<db>&p=<password>&u=<user>"
	  Additional parameter -M time:unix:usec:utc for correct timestamps in InfluxDB recommended
	Specify host/port for syslog with e.g. -F syslog:127.0.0.1:1514


		= メタ情報オプション =
  [-M time[:<オプション>]|protocol|level|noise[:<secs>]|stats|bits] Add various metadata to every output line.
	Use "time" to add current date and time meta data (preset for live inputs).
	Use "time:rel" to add sample position meta data (preset for read-file and stdin).
	Use "time:unix" to show the seconds since unix epoch as time meta data.
	Use "time:iso" to show the time with ISO-8601 format (YYYY-MM-DD"T"hh:mm:ss).
	Use "time:off" to remove time meta data.
	Use "time:usec" to add microseconds to date time meta data.
	Use "time:tz" to output time with timezone offset.
	Use "time:utc" UTC で時間を出力する。
		(this may also be accomplished by invocation with TZ environment variable set).
		"usec" and "utc" can be combined with other options, eg. "time:unix:utc:usec".
	Use "protocol" / "noprotocol" to output the decoder protocol number meta data.
	Use "level" to add Modulation, Frequency, RSSI, SNR, and Noise meta data.
	Use "noise[:secs]" to report estimated noise level at intervals (default: 10 seconds).
	Use "stats[:[<level>][:<interval>]]" to report statistics (default: 600 seconds).
	  level 0: no report, 1: report successful devices, 2: report active devices, 3: report all
	Use "bits" to add bit representation to code outputs (for debug).


		= 読み込みファイルオプション =
  [-r <ファイル名>] 受信機ではなく、入力ファイルからデータを読み込む。
	Parameters are detected from the full path, file name, and extension.

	A center frequency is detected as (fractional) number suffixed with 'M',
	'Hz', 'kHz', 'MHz', or 'GHz'.

	A sample rate is detected as (fractional) number suffixed with 'k',
	'sps', 'ksps', 'Msps', or 'Gsps'.

	File content and format are detected as parameters, possible options are:
	'cu8', 'cs16', 'cf32' ('IQ' implied), and 'am.s16'.

	Parameters must be separated by non-alphanumeric chars and are case-insensitive.
	Overrides can be prefixed, separated by colon (':')

	E.g. default detection by extension: path/filename.am.s16
	forced overrides: am:s16:path/filename.ext

	Reading from pipes also support format options.
	例： reading complex 32-bit float: CU32:-


		= 書き込みファイルオプション =
  [-w <ファイル名>] データストリームを出力ファイルに保存する。(a '-' dumps samples to stdout)
  [-W <ファイル名>] データストリームを出力ファイルに保存し、既に存在するファイルに上書きする。
	Parameters are detected from the full path, file name, and extension.

	File content and format are detected as parameters, possible options are:
	'cu8', 'cs8', 'cs16', 'cf32' ('IQ' implied),
	'am.s16', 'am.f32', 'fm.s16', 'fm.f32',
	'i.f32', 'q.f32', 'logic.u8', 'ook', and 'vcd'.

	Parameters must be separated by non-alphanumeric chars and are case-insensitive.
	Overrides can be prefixed, separated by colon (':')

	例： default detection by extension: path/filename.am.s16
	forced overrides: am:s16:path/filename.ext

```


使用例：

| コマンド | 詳細
|---------|------------
| `rtl_433` | デフォルトの受信モードであり、最初に見つかったデバイスを使用して 433.92 MHz を、サンプリングレート 250k で受信します。
| `rtl_433 -C si` | Default receive mode, also convert units to metric system.
| `rtl_433 -f 868M -s 1024k` | 868 MHz を、サンプリングレート 1024k で受信します。
| `rtl_433 -M hires -M level` | Report microsecond accurate timestamps and add reception levels (depending on gain).
| `rtl_433 -R 1 -R 8 -R 43` | 指定したデバイスに対してのみ復調を有効にする。
| `rtl_433 -A` | パルス解析を有効にします。パルスのタイミング、空白、期間を要約し表示します。Can be used with `-R 0` to disable decoders.
| `rtl_433 -S all -T 120` | 検出したすべての信号を次の様式 (`g###_###M_###k.cu8`)で保存し、2分間動作します。
| `rtl_433 -K FILE -r file_name` | 受信機ではなく、入力ファイルからデータを読み込む。 Tag output with filenames.
| `rtl_433 -F json -M utc \| mosquitto_pub -t home/rtl_433 -l` | Will pipe the output to network as JSON formatted MQTT messages. A test MQTT client can be found in `examples/mqtt_rtl_433_test_client.py`.
| `rtl_433 -f 433.53M -f 434.02M -H 15` | 15秒のホッピング間隔で、2つの周波数をポーリングする。

## Googleグループ

rtl_433について更なる情報が欲しい方は、Googleグループに参加しましょう。：
https://groups.google.com/forum/#!forum/rtl_433


## トラブルシューティング

下記のエラーが表示された場合:

    Kernel driver is active, or device is claimed by second instance of librtlsdr.
    In the first case, please either detach or blacklist the kernel module
    (dvb_usb_rtl28xxu), or enable automatic detaching at compile time.

その時は、

    sudo rmmod dvb_usb_rtl28xxu rtl2832

を行ってください。

## リリース

バージョンの表記法は、 年.月.　とします。 努めて API は各リリース間での互換性を保つようにしますが、あくまでもメンテナンス性を優先します。
