# digital-design
final project

### modile
```verilog
module t1(
				output reg [7:0] DATA_G, DATA_B, DATA_R, 
				output reg [6:0] SEG,
				output reg [3:0] COMM, COM,
				output reg [2:0] BLOOD,
				output reg piano_out,
				output dot,
				input CLK, L, R, START, STOP, FIRE
);
```

### 變數宣告
```
assign dot=0;
reg fire=0, w=0; 
reg clear=0, fail=0, up=1, r_bound=0,m_bound=1;
reg seg2=1, seg3=1, seg4=1; 
reg [2:0] board_l, board_m, board_r;
parameter   logic [7:0] wait_start [7:0] = 
          '{
            8'b11010111,
            8'b10111011,
            8'b10111101,
            8'b11011110,
            8'b11011110,
            8'b10111101,
            8'b10111011,
            8'b11010111
          };
        logic [2:0] block0 [7:0] = 
          '{
            3'b111,
            3'b111,
            3'b111,
            3'b111,
            3'b111,
            3'b111,
            3'b111,
            3'b111
          };
        logic [2:0] block1_start [7:0] = 
          '{
            3'b111,
            3'b111,
            3'b001,
            3'b001,
            3'b001,
            3'b001,
            3'b111,
            3'b111
          };
        logic [2:0] block1[7:0] = 
          '{
            3'b111,
            3'b111,
            3'b001,
            3'b001,
            3'b001,
            3'b001,
            3'b111,
            3'b111
          };
        logic [2:0] block2_start [7:0] = 
          '{
            3'b111,
            3'b111,
            3'b111,
            3'b101,
            3'b111,
            3'b111,
            3'b111,
            3'b111
          };
        logic [2:0] block2 [7:0] = 
        '{
            3'b111,
            3'b111,
            3'b111,
            3'b101,
            3'b111,
            3'b111,
            3'b111,
            3'b111
          };

        logic [7:0] wait_fail [7:0] =
          '{
            8'b11111111,
            8'b10001111,
            8'b10001011,
            8'b10001101,
            8'b11111011,
            8'b10001101,
            8'b10001011,
            8'b10001111
          };
        logic [6:0] seg [3:0] = 
          '{
            7'b0000001,
            7'b0000001,
            7'b0000001,
            7'b0000001
          };
        logic [6:0] wait_seg [3:0] = 
          '{
            7'b0000001,
            7'b0000001,
            7'b0000001,
            7'b0000001
          };
        logic [7:0] wait_ffp [111:0] =
          '{
            8'b11111111,
            8'b11111111,
            8'b11111111,
            8'b11111111,
            8'b11111111,
            8'b11111111,
            8'b11111111,
            8'b11011011,
            8'b00000011,
            8'b11011111,
            8'b11111111,
            8'b11011011,
            8'b11011011,
            8'b11000011,
            8'b11111111,
            8'b11011011,
            8'b10101011,
            8'b10101011,
            8'b11000111,
            8'b11111111,
            8'b10100000,
            8'b11111110,
            8'b11111111,
            8'b11000011,
            8'b11011011,
            8'b11000011,
            8'b11111111,
            8'b11011111,
            8'b11011111,
            8'b11000011,
            8'b11111111,
            8'b10011111,
            8'b01101111,
            8'b01101111,
            8'b00000011,//project
            8'b11111111,
            8'b11111111,
            8'b00000011,
            8'b11111111,
            8'b11111011,
            8'b11100111,
            8'b11011011,
            8'b11011011,
            8'b11100111,
            8'b11111111,
            8'b11000011,
            8'b11011111,
            8'b11000011,
            8'b11111111,
            8'b10100011,
            8'b11111111,
            8'b01111111,
            8'b01101111,
            8'b01101111,
            8'b00000011, //final
            8'b11111111,
            8'b11111111,
            8'b11000001,
            8'b10110110,
            8'b10110110,
            8'b11000111,
            8'b11111111,
            8'b11000011,
            8'b11011111,
            8'b11000011,
            8'b11111111,
            8'b10100011,
            8'b11111111,
            8'b11011011, //k
            8'b11100111,
            8'b00000011,
            8'b11111111,
            8'b11101011,
            8'b11110111,
            8'b11101011,
            8'b11111111,
            8'b11101011,
            8'b11110111,
            8'b11101011,
            8'b01111111,
            8'b01101111,
            8'b01101111,
            8'b00000011,
            8'b11111111,
            8'b11111111,
            8'b11011111,
            8'b11011111,
            8'b11000011,
            8'b11111111,
            8'b11000011,
            8'b11111011,
            8'b11000011,
            8'b11111111,
            8'b11000011,
            8'b11011011,
            8'b11000011,
            8'b01111111,
            8'b10011111,
            8'b11011111,
            8'b11000011,
            8'b10011111,
            8'b01111111,
            8'b11111111,
            8'b11111111,
            8'b11000011,
            8'b11011011,
            8'b11000011,
            8'b01100011,
            8'b01101011,
            8'b01101011,
            8'b01111011,
            8'b00000011
          };
          bit [2:0] cnt;
          bit [1:0] cntseg;
          bit [2:0] ball_x, ball_y;
          bit [7:0] horse;
```

### Clock設定
```
divfreq3 T0(CLK, CLK_10k);
divfreq1 T1(CLK, CLK_1hz);
divfreq2 T2(CLK, CLK_10hz);
```

### 初始化
```
initial
			begin
				cnt = 0;
				DATA_R = 8'b11111111; 
				DATA_G = 8'b11111111;
				DATA_B = 8'b11111111;
				COMM = 4'b1000;  
				COM = 4'b1110; 
				SEG = 7'b0000001; 
				board_l = 3'b010;
				board_m = 3'b011;
				board_r = 3'b100;
				m_bound=1;
				ball_x = 3'b011;
				ball_y = 3'b001;
				BLOOD=3'b111;
			end
```

### 計時器
```
always @(posedge CLK_1hz)
			begin
				if(START && ~STOP && ~clear && ~fail)begin
					case(seg[0])
						7'b0000001:
							begin
								seg[0] <= 7'b1001111;
								seg2 = ~seg2;
							end
						7'b1001111: seg[0] <= 7'b0010010;
						7'b0010010: seg[0] <= 7'b0000110;
						7'b0000110: seg[0] <= 7'b1001100;
						7'b1001100: seg[0] <= 7'b0100100;
						7'b0100100: seg[0] <= 7'b0100000;
						7'b0100000: seg[0] <= 7'b0001111;
						7'b0001111: seg[0] <= 7'b0000000;
						7'b0000000: seg[0] <= 7'b0000100;
						7'b0000100:	
							begin
								seg[0] <= 7'b0000001;
								seg2 = ~seg2; //carry to seg2
							end							
					endcase
				end
				else if(~START)
					begin
						seg[0] = 7'b0000001;
						seg2=1;
					end

			end
		
		always @(posedge seg2)
			begin
				if(START)begin
					case(seg[1])
						7'b0000001: 
							begin
								seg3 = ~seg3;
								seg[1] <= 7'b1001111;
							end
						7'b1001111: seg[1] <= 7'b0010010;
						7'b0010010: seg[1] <= 7'b0000110;
						7'b0000110: seg[1] <= 7'b1001100;
						7'b1001100: seg[1] <= 7'b0100100;
						7'b0100100:
							begin
								seg[1] <= 7'b0000001;
								seg3 = ~seg3;
							end
					endcase
				end
				else if(~START)
					begin
						seg[1] = 7'b0000001;
						seg3=1;
					end
			end
			
		always @(posedge seg3)
			begin
				if(START)begin
					case(seg[2])
						7'b0000001:
							begin
								seg[2] <= 7'b1001111;
								seg4 = ~seg4;
							end
						7'b1001111: seg[2] <= 7'b0010010;
						7'b0010010: seg[2] <= 7'b0000110;
						7'b0000110: seg[2] <= 7'b1001100;
						7'b1001100: seg[2] <= 7'b0100100;
						7'b0100100: seg[2] <= 7'b0100000;
						7'b0100000: seg[2] <= 7'b0001111;
						7'b0001111: seg[2] <= 7'b0000000;
						7'b0000000: seg[2] <= 7'b0000100;
						7'b0000100: 
							begin
									seg[2] <= 7'b0000001;
									seg4 = ~seg4;
							end								
					endcase
				end
				else if(~START)
					begin
						seg[2] = 7'b0000001;
						seg4=1;
					end
			end

		always @(posedge seg4)
			begin
				if(START)begin
					case(seg[3])
						7'b0000001: seg[3] <= 7'b1001111;
						7'b1001111: seg[3] <= 7'b0010010;
						7'b0010010: seg[3] <= 7'b0000110;
						7'b0000110: seg[3] <= 7'b1001100;
						7'b1001100: seg[3] <= 7'b0100100;
						7'b0100100: seg[3] <= 7'b0000001;
					endcase
				end
				else if(~START)
					seg[3] = 7'b0000001;
			end
```

### 射擊按鈕觸發
```
always @(negedge FIRE)
			fire = ~fire;
```

### 主要按鍵控制(左右、START、暫停)
```
always @(posedge CLK_10hz)
			begin 
				if(horse>=103 || ~fail) horse=0;
				else horse = horse +1;
				//move the board, cannot move when board touch the border
				if(START && ~STOP && ~clear && ~fail)begin
					if(L==1 && board_l != 3'b000)	begin
						board_l=board_l-1'b1;
						board_m=board_m-1'b1;
						board_r=board_r-1'b1;
					end
					if(R==1 && board_r != 3'b111)	begin
						board_l=board_l+1'b1;
						board_m=board_m+1'b1;
						board_r=board_r+1'b1;
					end
					if(~fire)begin w=0;	ball_x=board_m;	ball_y = 3'b001; up=1;end
					if(fire)begin
						if(up)begin
							if(ball_y<7)	ball_y=ball_y+1;
							else begin
								ball_y=ball_y-1; up=~up; 
							end
						end
						else begin //down
							if(ball_y>1)ball_y=ball_y-1;
							else if(ball_y==1 && ball_x==board_l) begin //go left
								ball_y=ball_y+1; 	up=~up;
								r_bound=0;	m_bound=0;
							end
							else if(ball_y==1 && ball_x==board_m) begin //go straight up
								ball_y=ball_y+1; m_bound=1;	up=~up;
							end
							else if(ball_y==1 && ball_x==board_r) begin //go right
								ball_y=ball_y+1;r_bound=1; m_bound=0;	up=~up;
							end
							else if(ball_y==1) begin
								BLOOD={BLOOD[1:0],1'b0}; //shift left 0
								if(BLOOD==3'b000)	fail=1;
								board_l = 3'b010;
								board_m = 3'b011;
								board_r = 3'b100;
								m_bound=1;
								ball_x = 3'b011;
								ball_y = 3'b001;
								w=1;
							end
						end
						//ball_x control
						if(r_bound && ~m_bound)begin 
							if(ball_x<7)	ball_x=ball_x+1;
							else begin
								ball_x=ball_x-1; 
								r_bound=~r_bound; 
							end
						end
						else if(~r_bound && ~m_bound)begin
							if(ball_x>0)	ball_x=ball_x-1;
							else begin
								ball_x=ball_x+1; 
								r_bound=~r_bound; 
							end
						end
						//block control
						for(integer t=0;t<8;t=t+1) begin //row
							for(integer h=0;h<3;h=h+1) begin //col								
								if(block2[t][h]==0 && ball_x==t && ball_y==(h+5) && up && r_bound && ~m_bound)begin //when ball on block2 and right in
									block2[t][h]=1; ball_x=ball_x+1; ball_y=ball_y-1;up=0;									
								end
								if(block2[t][h]==0 && ball_x==t && ball_y==(h+5) && up && ~r_bound && ~m_bound)begin 
									block2[t][h]=1; ball_x=ball_x-1; ball_y=ball_y-1;up=0;
								end
								if(block2[t][h]==0 && ball_y==(h+4) && ball_x==t && up)begin block2[t][h]=1; up=0; end //ball under block
								if(block2[t][h]==1 && block1[t][h]==0 && ball_y==(h+4) && ball_x==t && up)begin 
									block1[t][h]=1; up=0;end
								if(block2[t][h]==1 && block1[t][h]==0 && ball_x==t && ball_y==(h+5) && up && r_bound && ~m_bound)begin 
									block1[t][h]=1; ball_x=ball_x+1; ball_y=ball_y-1; up=0;
								end
								if(block2[t][h]==1 && block1[t][h]==0 && ball_x==t && ball_y==(h+5) && up && ~r_bound && ~m_bound)begin 
									block1[t][h]=1; ball_x=ball_x-1; ball_y=ball_y-1;up=0;
								end
								//ball down
								if(block2[t][h]==0 && ball_x==t && ball_y==(h+5) && ~up && r_bound && ~m_bound)begin
									block2[t][h]=1; r_bound=~r_bound;  ball_x=ball_x-1; ball_y=ball_y-1;end
								if(block2[t][h]==0 && ball_x==t && ball_y==(h+5) && ~up && ~r_bound && ~m_bound)begin 
									block2[t][h]=1; r_bound=~r_bound; ball_x=ball_x+1; ball_y=ball_y-1;end
								if(block2[t][h]==1 && block1[t][h]==0 && ball_x==t && ball_y==(h+5) && ~up && r_bound && ~m_bound)begin 
									block1[t][h]=1; r_bound=~r_bound; ball_x=ball_x-1; ball_y=ball_y-1;end
								if(block2[t][h]==1 && block1[t][h]==0 && ball_x==t && ball_y==(h+5) && ~up && ~r_bound && ~m_bound)begin 
									block1[t][h]=1; r_bound=~r_bound; ball_x=ball_x+1; ball_y=ball_y-1;end

								if(block2[ball_x][ball_y-5]==0 && ball_y>4 && ~up) begin 
									block2[ball_x][ball_y-5]=1;
									if(r_bound) ball_x=ball_x-2;
									else ball_x=ball_x+2;
									r_bound=~r_bound;
								end
								if(block2[ball_x][ball_y-5]==1&& block1[ball_x][ball_y-5]==0 && ball_y>4 && ~up) begin
									block1[ball_x][ball_y-5]=1;
									if(r_bound) ball_x=ball_x-2;
									else ball_x=ball_x+2;									
									r_bound=~r_bound;
								end
								if(block1==block0 && block2==block0) clear=1;
							end
						end
					end
				end
				else if(~START)	begin
					board_l = 3'b010;
					board_m = 3'b011;
					board_r = 3'b100;
					clear=0;
					ball_x = 3'b011;
					ball_y = 3'b001;
					BLOOD=3'b110;
					fail=0;
					up=1;
					m_bound=1;
					block1=block1_start;
					block2=block2_start;
				end
			end
```

### 畫面顯示
```
always @(posedge CLK_10k) //updata screen
			begin
				if(cnt >= 7)	cnt = 0;
				else	cnt = cnt + 1;
				if(cntseg >= 3)	cntseg = 0;
				else	cntseg = cntseg + 1;
				COM = {COM[2:0], COM[3]};
				COMM = {1'b1, cnt};
				if(START && ~clear && ~fail && ~w)	begin
					DATA_B = {block2[cnt], 5'b11111};
					DATA_G = {block1[cnt], 5'b11111};
					if((board_l==cnt||board_m==cnt||board_r==cnt)&&ball_x==cnt)begin
						DATA_R = 8'b11111110;
						DATA_R[ball_y] = 1'b0;						
					end
					else if((board_l==cnt||board_m==cnt||board_r==cnt)&&ball_x!=cnt)
						DATA_R = 8'b11111110;
					else if(~(board_l==cnt||board_m==cnt||board_r==cnt)&&ball_x==cnt)begin
						DATA_R = 8'b11111111;
						DATA_R[ball_y] = 1'b0;						
					end
					else
						DATA_R = 8'b11111111;
					SEG = seg[cntseg];
				end
				else if(~START)	begin
					DATA_R = wait_start[cnt];
					DATA_B = wait_start[cnt];
					DATA_G = 8'b11111111;
					SEG = wait_seg[cntseg];
				end
				else if(fail) begin
					DATA_R = wait_ffp[horse+cnt];
					DATA_B = 8'b11111111;
					DATA_G = 8'b11111111;
					SEG = wait_seg[cntseg];
				end
				else if(w) begin
					DATA_R = wait_fail[cnt];
					DATA_B = wait_fail[cnt];
					DATA_G = 8'b11111111;
					SEG = wait_seg[cntseg];
				end
				else if(clear)	begin
					DATA_R = wait_start[cnt];
					DATA_B = 8'b11111111;
					DATA_G = 8'b11111111;
					SEG = seg[cntseg];
				end
			end
```

### 調整Clock
```
module divfreq3(input CLK, output reg CLK_10k);
	reg [24:0] count;
	always @(posedge CLK)
		begin
			if(count > 25000)
				begin
					count <= 25'b0;
					CLK_10k <= ~CLK_10k;
				end
			else
				count <= count + 1'b1;
		end
endmodule

module divfreq2(input CLK, output reg CLK_10hz);
	reg [24:0] count;
	always @(posedge CLK)
		begin
			if(count > 12500000)
				begin
					count <= 25'b0;
					CLK_10hz <= ~CLK_10hz;
				end
			else
				count <= count + 1'b1;
		end
endmodule

module divfreq1(input CLK, output reg CLK_1hz);
	reg [24:0] count;
	always @(posedge CLK)
		begin
			if(count > 25000000)
				begin
					count <= 25'b0;
					CLK_1hz <= ~CLK_1hz;
				end
			else
				count <= count + 1'b1;
		end
endmodule
```
