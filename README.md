Muhammad Rifqi Fadhilah - 5025211228

## TCP 

1. What is the IP address and TCP port number used by the client computer (source) that is transferring the alice.txt file to gaia.cs.umass.edu?

Answer : <img width="960" alt="1" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/a2ad30d8-6c6a-4a7e-9ff3-f7481dd6c928">

IP Source = `192.168.86.68` dan Source Port = `55639`

2. What is the IP address of gaia.cs.umass.edu? On what port number is it sending and receiving TCP segments for this connection?

Answer : IP Source = `192.119.245.12` dan Source Port = `80`

3. What is the sequence number of the TCP SYN segment that is used to initiate the TCP connection between the client computer and gaia.cs.umass.edu? What is it in this TCP segment that identifies the segment as a SYN segment? Will the TCP receiver in this session be able to use Selective Acknowledgments (allowing TCP to function a bit more like a “selective repeat” receiver, see section 3.4.5 in the text)?

Answer :  <img width="960" alt="3" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/5417970b-f300-4f71-986b-5a2347816ba2">

Sequence Number: `0` dan Sequence Number (raw) : `4236649187`, Karena memiliki Flags: `0x002 (SYN)`, Iya,  flag `SYN: Set` Ini berarti paket tersebut merupakan SYN segment, yang dapat digunakan untuk selective acknowledgments.

4. What is the sequence number of the SYNACK segment sent by gaia.cs.umass.edu to the client computer in reply to the SYN? What is it in the segment that identifies the segment as a SYNACK segment? What is the value of the Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu determine that value?

Answer : <img width="960" alt="4" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/3b1cc4f0-8eb3-441a-8010-bbd6bc20ea05">

- Sequence Number: `0` dan Sequence Number (raw) : `1068969752`
- Because has Flags : `0x012 (SYN, ACK)`
- Acknowledgment number : `4236649188`
- Acknowledgement number didapatkan dari sequence number dari SYN segment ditambah 1 `(ACK=Seq no+1)`.

5. What is the sequence number of the TCP segment containing the header of the HTTP POST command? Note that in order to find the POST message header, you’ll need to dig into the packet content field at the bottom of the Wireshark window, looking for a segment with the ASCII text “POST” within its DATA field4,5. How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file alice.txt fit into this single segment?

Answer : <img width="960" alt="5" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/655cc794-a430-450a-9fb7-aec25f7196f0">

- the HTTP POST segment sequence number = `152041` or sequence number (raw) = `4236801228`
- TCP payload `(1385 bytes)`
- Data dalam paket menunjukkan bahwa paket tersebut menggunakan multipart atau `MIME`, menandakan bahwa data yang dikirim terdiri dari beberapa segmen, bukan hanya satu segmen. Payload yang dikirim berukuran `1385 byte`, sedangkan ukuran alice.txt adalah `149KB`.

6. Consider the TCP segment containing the HTTP “POST” as the first segment in
the data transfer part of the TCP connection.
- At what time was the first segment (the one containing the HTTP POST) in
the data-transfer part of the TCP connection sent?
- At what time was the ACK for this first data-containing segment received?
- What is the RTT for this first data-containing segment?
- What is the RTT value the second data-carrying TCP segment and its ACK?

Answer : <img width="960" alt="6" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/6848fdfc-ea52-40de-813c-d0b794ad8701">

- At what time was the first segment (the one containing the HTTP POST) in
the data-transfer part of the TCP connection sent?

<img width="960" alt="6 b" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/c1ac3fda-a7af-4319-9edd-4185ddac7ab9">

berdasarkan gambar di atas, dapat dilihat bahwa paket pertama dikirim pada frame ke empat, dan dikirim pada `0.24047` seconds.

- At what time was the ACK for this first data-containing segment received?

<img width="960" alt="6 c" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/0066951d-c5f9-4fa3-9e14-b85545741fda">

Berdasarkan gambar di atas, ACK dari packet pertama diterima pada frame ke 7 dan frame diterima pada `0.052671` seconds

- What is the RTT for this first data-containing segment?
RTT dari segmen pertama adalah `0.028624` seconds

- What is the RTT value the second data-carrying TCP segment and its ACK?
  <img width="956" alt="6 d" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/ef2b3884-bb7c-4b5a-bb13-c14342fefe5a">

RTT dari segmen kedua adalah `0.028628` seconds

7. What is the length (header plus payload) of each of the first four data-carrying TCP segments?

Answer : 
`4` * (`Payload` + `Header`) = `4` * (`1448` + `32`) = `4` * `1480` = `5920 byte`

8. What is the minimum amount of available buffer space advertised to the client by gaia.cs.umass.edu among these first four data-carrying TCP segments7? Does the lack of receiver buffer space ever throttle the sender for these first four data- carrying segments?

Answer : 
<img width="960" alt="8" src="https://github.com/rifqol/hands-on-tcp-udp/assets/130858750/4626ca0f-c5d4-4371-909f-6483fcfc8d73">

Dari gambar layar yang diberikan, terlihat bahwa ada setidaknya `131712` ruang buffer yang tersedia, dihitung dari `window size value`. Selain itu, tidak pernah terjadi perlambatan pengiriman oleh penerima berdasarkan tangkapan layar karena `window size value` selalu lebih besar daripada `panjang` segmen yang dikirimkan.

9. Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?

Answer : 
- iya, ada
- Retransmitted segments dapat dideteksi melalui sequence number. Saat mengirim ulang, ada paket di mana sequence number dari paket berikutnya tidak lebih besar daripada sequence number dari paket sebelumnya.

10. How much data does the receiver typically acknowledge in an ACK among the first ten data-carrying segments sent from the client to gaia.cs.umass.edu? Can you identify cases where the receiver is ACKing every other received segment (see Table 3.2 in the text) among these first ten data-carrying segments?
Answer :
- `1448 byte`
- If the data is doubled, the acking segment for each segment is received, for example in the second segment the data is doubled from `1448` to `2896` bytes
