

// Verrtical Kernel 3a3
//-1 -2 -1 
// 0  0  0 
// 1  2  1

// Horizontal Kernel 3a3
//-1  0  1
//-2  0  2 
//-1  0  1

// ram address
// -----------
//            c0    c1    c2    c3 ...  c639
//         -+---------------------------------
// line0:   + p0	p1	  p2 	p3 ...  p639
// line1:   + p0 	p1 	  p2 	p3 ...  p639
// line2:   + p0 	p1 	  p2 	p3 ...  p639
// ...
// ...
// line438: + p0 	p1 	  p2 	p3 ...  p639
// line439: + p0 	p1 	  p2 	p3 ...  p639
//         -+---------------------------------

// ram_addr = h_cnt + v_cnt*640;

module convolution (
	input wire   	clock,
	input wire   	reset,
	input wire   	start,
	input wire 	 [3:0]	gray_pixel,
	output wire  [18:0] ram_addr,
	output wire  	ram_read,
	output wire  	ram_write,
	output wire 	g_cal,
	output wire 	done
);

// VGA: 640 x 480
reg [9:0] 	h_cnt;
reg [8:0] 	v_cnt;

wire [18:0] ram_addr; // 1pixel 4 bit, gray image;


// h_cnt
always @ (posedge clock) begin 
	if (reset == 1'b1 ) begin
		h_cnt <= 10'd0;
	end
	else begin
		if ( ram_read == 1'b1 )  begin 
			if (h_cnt == 10'd639) h_cnt <= 	10'd0;
			else                  h_cnt <=	h_cnt + 10'd1;
	end
end


// v_cnt
always @ (posedge clock) begin 
	if (reset == 1'b1 ) begin
		v_cnt <= 9'd0;
	end
	else begin
		if (v_cnt == 9'd479) begin 
			if (h_cnt == 10'd639) v_cnt <= 	9'd0;
		    else                  v_cnt <=	v_cnt;
		end
		else begin 
			if (h_cnt == 10'd639) v_cnt <= 	v_cnt + 9'd1;
		    else                  v_cnt <=	v_cnt;
		end
	end
end

// address 
assign ram_addr = v_cnt*640 + h_cnt;


// to calculate the edge factor
function g_cal;
    input [3:0] a1, a2, a3, a4, a5, a6, a7, a8 ,a9;
    reg   [5:0] y1, y2, x1, x2, gx_abs, gx_abs, g;   
		begin 
			// cal y 
			y1 = a1 + a2*2 + a3;
			y2 = a7 + a8*2 + a9;
			// cal x
			x1 = a1 + a4*2 + a7;
			x2 = a3 + a6*2 + a9;
			// cal gx, gy
			gy_abs = (y2 >= y1) ? (y2 - y1) : (y1 - y2);
			gx_abs = (x2 >= x1) ? (x2 - x1) : (x1 - x2);
			// cal g
			g_cal = (gy_abs + gx_abs);
		end
endfunction
	
endmodule


