<div align="center">

## Win\-98 Boot Sector Code


</div>

### Description

Hey guys, ever wondered how win9x loads ?.. well check out the disassembled, boot sector code for win98, which I happened to hack using a tool created by me called wrdsk which can be downloaded from psc. Please vote and post your comments so I can know that there are people interested in stuff like this... ;-)
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[vivek mohan](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/vivek-mohan.md)
**Level**          |Intermediate
**User Rating**    |4.0 (16 globes from 4 users)
**Compatibility**  |C
**Category**       |[System Services/ Functions](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/system-services-functions__3-23.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/vivek-mohan-win-98-boot-sector-code__3-3845/archive/master.zip)





### Source Code

<h1>The Windows-98 Boot Sector Code</h1>
Hi there ! Thought may be there'd people out there who'd be interested in stuff like this. I used the program wrdisk.exe , created by me, (check out my <a href="http://www.geocities.com/">c/c++ resource center</a>, or download it from <a href="">PSC</a>) to read the 0th head, 0th track, 1st sector of my hardisk and Behold .. I found this.... ;-). The actual binary data as follows...
<br><br>
<p>
<center>
<table width="70%">
<td bgcolor="#ffeeee">
<font face="courier new" size="2">
3ÀŽÐ¼ |ûPPü¾|¿PW¹åó¤Ë¾¾±8,|	uƒÆâõÍ‹‹îƒÆIt8,tö¾N¬< tú» ´Íëò‰F%–ŠF´<t´<t:Äu+@ÆF%u$»ªUP´AÍXrûUªuöÁtŠàˆV$Ç¡ëˆf¿
 ¸‹Ü3Éƒÿ‹N%NÍr)¾u>þ}UªtZƒïÚ…öuƒ¾?ëŠ˜‘R™FV
è ZëÕOtä3ÀÍë¸ €8V3öVVRPSQ¾ V‹ôPR¸ BŠV$ÍZXdr
@uB€Çâ÷ø^ÃëtInvalid partition table. Setup cannot continue. Error loading operating system. Setup cannot continue.       ‹üW‹õËot from floppy...      | ]   €
   ù?€ þ?¶?  ¸Û,  ·þ¿
÷Û, TXS                 Uª
</font>
</td>
</table>
</p>
<p>
Thats 512 bytes of Mumbo-Jumbo....but it made sense when I disassembled it using <a href="http://web-sites.co.uk/nasm/">NASM</a>. This is what I got....
<br>
<i>I'm trying to heavily comment this code, watch this space..</i>
<table width="70%">
<td bgcolor="#ffeeee">
<pre>
00000000 33C0       xor ax,ax
00000002 8ED0       mov ss,ax
00000004 BC007C      mov sp,0x7c00
00000007 FB        sti
00000008 50        push ax
00000009 07        pop es
0000000A 50        push ax
0000000B 1F        pop ds
0000000C FC        cld
0000000D BE1B7C      mov si,0x7c1b
00000010 BF1B06      mov di,0x61b
00000013 50        push ax
00000014 57        push di
00000015 B9E501      mov cx,0x1e5
00000018 F3A4       rep movsb
0000001A CB        retf
0000001B BEBE07      mov si,0x7be
0000001E B104       mov cl,0x4
00000020 382C       cmp [si],ch
00000022 7C09       jl 0x2d
00000024 7515       jnz 0x3b
00000026 83C610      add si,byte +0x10
00000029 E2F5       loop 0x20
0000002B CD18       int 0x18
0000002D 8B14       mov dx,[si]
0000002F 8BEE       mov bp,si
00000031 83C610      add si,byte +0x10
00000034 49        dec cx
00000035 7416       jz 0x4d
00000037 382C       cmp [si],ch
00000039 74F6       jz 0x31
0000003B BE1007      mov si,0x710
0000003E 4E        dec si
0000003F AC        lodsb
00000040 3C00       cmp al,0x0
00000042 74FA       jz 0x3e
00000044 BB0700      mov bx,0x7
00000047 B40E       mov ah,0xe
00000049 CD10       int 0x10
0000004B EBF2       jmp short 0x3f
0000004D 894625      mov [bp+0x25],ax
00000050 96        xchg ax,si
00000051 8A4604      mov al,[bp+0x4]
00000054 B406       mov ah,0x6
00000056 3C0E       cmp al,0xe
00000058 7411       jz 0x6b
0000005A B40B       mov ah,0xb
0000005C 3C0C       cmp al,0xc
0000005E 7405       jz 0x65
00000060 3AC4       cmp al,ah
00000062 752B       jnz 0x8f
00000064 40        inc ax
00000065 C6462506     mov byte [bp+0x25],0x6
00000069 7524       jnz 0x8f
0000006B BBAA55      mov bx,0x55aa
0000006E 50        push ax
0000006F B441       mov ah,0x41
00000071 CD13       int 0x13
00000073 58        pop ax
00000074 7216       jc 0x8c
00000076 81        db 0x81
00000077 FB        sti
00000078 55        push bp
00000079 AA        stosb
0000007A 7510       jnz 0x8c
0000007C F6C101      test cl,0x1
0000007F 740B       jz 0x8c
00000081 8AE0       mov ah,al
00000083 885624      mov [bp+0x24],dl
00000086 C706A106EB1E   mov word [0x6a1],0x1eeb
0000008C 886604      mov [bp+0x4],ah
0000008F BF0A00      mov di,0xa
00000092 B80102      mov ax,0x201
00000095 8BDC       mov bx,sp
00000097 33C9       xor cx,cx
00000099 83FF05      cmp di,byte +0x5
0000009C 7F03       jg 0xa1
0000009E 8B4E25      mov cx,[bp+0x25]
000000A1 034E02      add cx,[bp+0x2]
000000A4 CD13       int 0x13
000000A6 7229       jc 0xd1
000000A8 BE7507      mov si,0x775
000000AB 81        db 0x81
000000AC 3E        db 0x3E
000000AD FE        db 0xFE
000000AE 7D55       jnl 0x105
000000B0 AA        stosb
000000B1 745A       jz 0x10d
000000B3 83EF05      sub di,byte +0x5
000000B6 7FDA       jg 0x92
000000B8 85F6       test si,si
000000BA 7583       jnz 0x3f
000000BC BE3F07      mov si,0x73f
000000BF EB8A       jmp short 0x4b
000000C1 98        cbw
000000C2 91        xchg ax,cx
000000C3 52        push dx
000000C4 99        cwd
000000C5 034608      add ax,[bp+0x8]
000000C8 13560A      adc dx,[bp+0xa]
000000CB E81200      call 0xe0
000000CE 5A        pop dx
000000CF EBD5       jmp short 0xa6
000000D1 4F        dec di
000000D2 74E4       jz 0xb8
000000D4 33C0       xor ax,ax
000000D6 CD13       int 0x13
000000D8 EBB8       jmp short 0x92
000000DA 0000       add [bx+si],al
000000DC 803810      cmp byte [bx+si],0x10
000000DF 125633      adc dl,[bp+0x33]
000000E2 F65656      not byte [bp+0x56]
000000E5 52        push dx
000000E6 50        push ax
000000E7 06        push es
000000E8 53        push bx
000000E9 51        push cx
000000EA BE1000      mov si,0x10
000000ED 56        push si
000000EE 8BF4       mov si,sp
000000F0 50        push ax
000000F1 52        push dx
000000F2 B80042      mov ax,0x4200
000000F5 8A5624      mov dl,[bp+0x24]
000000F8 CD13       int 0x13
000000FA 5A        pop dx
000000FB 58        pop ax
000000FC 8D6410      lea sp,[si+0x10]
000000FF 720A       jc 0x10b
00000101 40        inc ax
00000102 7501       jnz 0x105
00000104 42        inc dx
00000105 80C702      add bh,0x2
00000108 E2F7       loop 0x101
0000010A F8        clc
0000010B 5E        pop si
0000010C C3        ret
0000010D EB74       jmp short 0x183
0000010F 49        dec cx
00000110 6E        outsb
00000111 7661       jna 0x174
00000113 6C        insb
00000114 69        db 0x69
00000115 64207061     and [fs:bx+si+0x61],dh
00000119 7274       jc 0x18f
0000011B 69        db 0x69
0000011C 7469       jz 0x187
0000011E 6F        outsw
0000011F 6E        outsb
00000120 207461      and [si+0x61],dh
00000123 626C65      bound bp,[si+0x65]
00000126 2E205365     and [cs:bp+di+0x65],dl
0000012A 7475       jz 0x1a1
0000012C 7020       jo 0x14e
0000012E 63616E      arpl [bx+di+0x6e],sp
00000131 6E        outsb
00000132 6F        outsw
00000133 7420       jz 0x155
00000135 636F6E      arpl [bx+0x6e],bp
00000138 7469       jz 0x1a3
0000013A 6E        outsb
0000013B 7565       jnz 0x1a2
0000013D 2E004572     add [cs:di+0x72],al
00000141 726F       jc 0x1b2
00000143 7220       jc 0x165
00000145 6C        insb
00000146 6F        outsw
00000147 61        popa
00000148 64        db 0x64
00000149 69        db 0x69
0000014A 6E        outsb
0000014B 67206F70     and [edi+0x70],ch
0000014F 657261      gs jc 0x1b3
00000152 7469       jz 0x1bd
00000154 6E        outsb
00000155 67207379     and [ebx+0x79],dh
00000159 7374       jnc 0x1cf
0000015B 656D       gs insw
0000015D 2E205365     and [cs:bp+di+0x65],dl
00000161 7475       jz 0x1d8
00000163 7020       jo 0x185
00000165 63616E      arpl [bx+di+0x6e],sp
00000168 6E        outsb
00000169 6F        outsw
0000016A 7420       jz 0x18c
0000016C 636F6E      arpl [bx+0x6e],bp
0000016F 7469       jz 0x1da
00000171 6E        outsb
00000172 7565       jnz 0x1d9
00000174 2E0000      add [cs:bx+si],al
00000177 0000       add [bx+si],al
00000179 0000       add [bx+si],al
0000017B 0000       add [bx+si],al
0000017D 0000       add [bx+si],al
0000017F 0000       add [bx+si],al
00000181 0000       add [bx+si],al
00000183 8BFC       mov di,sp
00000185 1E        push ds
00000186 57        push di
00000187 8BF5       mov si,bp
00000189 CB        retf
0000018A 6F        outsw
0000018B 7420       jz 0x1ad
0000018D 66726F      o32 jc 0x1ff
00000190 6D        insw
00000191 20666C      and [bp+0x6c],ah
00000194 6F        outsw
00000195 7070       jo 0x207
00000197 792E       jns 0x1c7
00000199 2E2E0000     add [cs:bx+si],al
0000019D 0000       add [bx+si],al
0000019F 0000       add [bx+si],al
000001A1 0010       add [bx+si],dl
000001A3 0001       add [bx+di],al
000001A5 0000       add [bx+si],al
000001A7 7C00       jl 0x1a9
000001A9 0008       add [bx+si],cl
000001AB 5D        pop bp
000001AC 0000       add [bx+si],al
000001AE 0000       add [bx+si],al
000001B0 0000       add [bx+si],al
000001B2 800000      add byte [bx+si],0x0
000001B5 0D0E00      or ax,0xe
000001B8 0000       add [bx+si],al
000001BA 0000       add [bx+si],al
000001BC F9        stc
000001BD 3F        aas
000001BE 800101      add byte [bx+di],0x1
000001C1 0006FE3F     add [0x3ffe],al
000001C5 B63F       mov dh,0x3f
000001C7 0000       add [bx+si],al
000001C9 00B8DB2C     add [bx+si+0x2cdb],bh
000001CD 0000       add [bx+si],al
000001CF 0001       add [bx+di],al
000001D1 B70B       mov bh,0xb
000001D3 FE        db 0xFE
000001D4 BF0AF7      mov di,0xf70a
000001D7 DB2C       fld tword [si]
000001D9 005458      add [si+0x58],dl
000001DC 53        push bx
000001DD 0000       add [bx+si],al
000001DF 0000       add [bx+si],al
000001E1 0000       add [bx+si],al
000001E3 0000       add [bx+si],al
000001E5 0000       add [bx+si],al
000001E7 0000       add [bx+si],al
000001E9 0000       add [bx+si],al
000001EB 0000       add [bx+si],al
000001ED 0000       add [bx+si],al
000001EF 0000       add [bx+si],al
000001F1 0000       add [bx+si],al
000001F3 0000       add [bx+si],al
000001F5 0000       add [bx+si],al
000001F7 0000       add [bx+si],al
000001F9 0000       add [bx+si],al
000001FB 0000       add [bx+si],al
000001FD 0055AA      add [di-0x56],dl
</pre>
</td>
</table>
<p>
I am not really experienced at all this so please <a href="mailto:opendev@phreaker.net">mail me</a> your suggestions, comments, death threats ;-)
</p>

