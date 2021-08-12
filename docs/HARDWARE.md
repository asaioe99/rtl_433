# rtl_433 で動作試験済みのデバイス

rtl_433 は以下のSDRデバイスで動作または試験されています:

## RTL-SDR

試験及びサポートが活発なのは、DVB dongles ベースの Realtek RTL2832 (及び RTL-SDR によりサポートされている同等のデバイス）です。

右記も参照のこと [RTL-SDR](https://github.com/osmocom/rtl-sdr/)

## SoapySDR

試験及びサポートが活発なのは、下記の通り。
- [LimeSDR USB](https://www.crowdsupply.com/lime-micro/limesdr)
- [LimeSDR mini](https://www.crowdsupply.com/lime-micro/limesdr-mini)
- [LimeNet Micro](https://www.crowdsupply.com/lime-micro/limenet-micro)
- [PlutoSDR](https://www.analog.com/en/design-center/evaluation-hardware-and-software/evaluation-boards-kits/adalm-pluto.html)
- [SDRplay](https://www.sdrplay.com/) (RSP1A tested)
- [HackRF One](https://greatscottgadgets.com/hackrf/) (実機不保有のため、報告のみ)
- [SoapyRemote](https://github.com/pothosware/SoapyRemote/wiki)

LimeSDR 及び LimeNet の技術サンプルは、[MyriadRf](https://myriadrf.org/) により提供して頂いてます。

右記も参照のこと [SoapySDR](https://github.com/pothosware/SoapySDR/)

## 未サポート

- Ultra cheap 1-bit (OOK) receivers, and antenna-on-a-raspi-pin
- CC1101, and alike special purpose / non general SDR chips
