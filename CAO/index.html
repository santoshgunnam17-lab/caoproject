import streamlit as st
import streamlit.components.v1 as components
import pandas as pd
import matplotlib.pyplot as plt
import math
import json
import random

st.set_page_config(page_title="Memory Interleaving & Address Mapping Visual Simulator", layout="wide")

st.markdown("""
<style>
.stApp { background-color:#0E1117; }
.header-box {
    padding:30px;background-color:#355E3B;
    border-radius:12px; text-align:center; color:white; border: 1px solid #2e7d32;
    margin-bottom: 20px;
}
h2 { color:#FF9800 !important; }
div[data-testid="stMetricValue"] { color:#FF9800; font-size:32px; }
</style>
<div class="header-box">
  <h1>Memory Interleaving & Address Mapping Visual Simulator</h1>
</div>
""", unsafe_allow_html=True)

def generate_addresses(pattern, base, n, word_size, capacity, stride_val):
    addresses = []
    base = (base // word_size) * word_size
    for i in range(n):
        if pattern == "Sequential": 
            addr = (base + i * word_size) % capacity
        elif pattern == "Stride": 
            addr = (base + i * stride_val * word_size) % capacity
        else:
            addr = random.randint(0, capacity - 1)
        addresses.append((addr // word_size) * word_size)
    return addresses

def simulate(addresses, word_size, banks, rows, bank_cycles, addr_bits):
    busy_until = [0] * banks
    logs = []
    current_cycle = 0
    conflicts = 0
    stall_cycles = 0

    for i, addr in enumerate(addresses, start=1):
        word_addr = addr // word_size
        bank = word_addr % banks
        row = (word_addr // banks) % rows
        start = max(current_cycle, busy_until[bank])
        
        is_conflict = "NO"
        if start > current_cycle:
            conflicts += 1
            stall_cycles += start - current_cycle
            is_conflict = "YES"
            
        end = start + bank_cycles
        busy_until[bank] = end

        logs.append({
            "Access": i, "Address(dec)": addr,
            "Address(bin)": format(addr, f'0{addr_bits}b'),
            "Bank": bank, "Row": row, 
            "StartCycle": start, "EndCycle": end,
            "Conflict": is_conflict
        })
        current_cycle += 1
        
    total_cycles = max(log["EndCycle"] for log in logs)
    return logs, total_cycles, conflicts, stall_cycles

def plot_timeline(logs, banks):
    fig, ax = plt.subplots(figsize=(10, 5))
    fig.patch.set_facecolor('#0E1117')
    ax.set_facecolor('#000')
    colors = ["#4CAF50", "#2196F3", "#FF9800", "#E91E63", "#9C27B0", "#00BCD4"]
    
    for entry in logs:
        b, s, dur = entry["Bank"], entry["StartCycle"], entry["EndCycle"] - entry["StartCycle"]
        ax.barh(b, dur, left=s, color=colors[b % len(colors)], edgecolor='white', linewidth=0.5)
        ax.text(s + dur/2, b, f"A{entry['Access']}", ha='center', va='center', color='white', fontsize=8)
        
    ax.set_yticks(range(banks))
    ax.set_yticklabels([f"Bank {b}" for b in range(banks)], color='white')
    ax.set_xlabel("Cycles", color='white')
    ax.tick_params(colors='white')
    ax.spines['bottom'].set_color('white')
    ax.spines['left'].set_color('white')
    return fig

def animate_hardware(logs, banks, addr_bits, bank_bits, row_bits, offset_bits, word_size, rows_per_bank, anim_speed):
    js_log = json.dumps(logs)
    speed_ms = {1: 5000, 2: 3500, 3: 2000, 4: 1200, 5: 600}[anim_speed]
    
    bank_label = f"{offset_bits + bank_bits - 1}-{offset_bits}" if bank_bits > 1 else f"{offset_bits}"
    row_label = f"{addr_bits - 1}-{offset_bits + bank_bits}" if row_bits > 1 else f"{addr_bits - 1}"

    chip_h = 100 if banks > 8 else 125
    chip_gap = 40 if banks > 8 else 60
    total_h = 500 + (banks * (chip_h + chip_gap))

    html_code = f"""
    <div style="background:#0a0e14; padding:20px; border-radius:12px; font-family: monospace; color: white; border: 1px solid #1a2a1a;">
        
        <div id="top-bar" style="display:flex; gap:10px; margin-bottom:15px; flex-wrap:wrap;">
            <div style="background:#111; border:1px solid #222; padding:6px 14px; border-radius:6px; font-size:13px; color:#90caf9">Banks: <b style="color:#FF9800">{banks}</b></div>
            <div style="background:#111; border:1px solid #222; padding:6px 14px; border-radius:6px; font-size:13px; color:#90caf9">Addr: <b style="color:#FF9800">{addr_bits}b</b> [Row {row_bits}|Bnk {bank_bits}|Off {offset_bits}]</div>
            <div style="background:#111; border:1px solid #222; padding:6px 14px; border-radius:6px; font-size:13px; color:#90caf9">Word: <b style="color:#FF9800">{word_size}B</b></div>
        </div>

        <div id="access-card" style="border:1px solid #00ff41; border-radius:10px; padding:15px; margin-bottom:20px; background:rgba(0,255,65,0.05); min-height:100px;">
            <div style="font-weight:bold; font-size:18px;">Access #<span id="step-cnt">1</span></div>
            <div style="margin-top:8px; font-size:15px; color:#aaa;">
                Addr: <b id="addr-dec" style="color:white">-</b> <b id="addr-hex" style="color:#FF9800">-</b> &nbsp; 
                Bank: <b id="bnk-val" style="color:#00ff41">-</b> &nbsp; 
                Row: <b id="row-val" style="color:white">-</b>
            </div>
            <div style="margin-top:12px; letter-spacing:3px; font-size:18px;">Binary: <span id="bin-str">-</span></div>
        </div>

        <div style="margin-bottom:15px; display:flex; gap:12px;">
            <button onclick="prevStep()" style="padding:10px 20px; background:#111; color:#00ff41; border:1px solid #00ff41; border-radius:4px; cursor:pointer; font-weight:bold;">PREV</button>
            <button id="playBtn" onclick="togglePlay()" style="padding:10px 20px; background:#1B5E20; color:white; border:none; border-radius:4px; cursor:pointer; font-weight:bold; width:120px;">PLAY ALL</button>
            <button onclick="nextStep()" style="padding:10px 20px; background:#111; color:#00ff41; border:1px solid #00ff41; border-radius:4px; cursor:pointer; font-weight:bold;">NEXT</button>
        </div>

        <canvas id="hwCanvas" width="950" height="{total_h}" style="background:#000;"></canvas>
    </div>
    
    <script>
        const logs = {js_log};
        const NB = {banks};
        const RB = {row_bits}, BB = {bank_bits}, OB = {offset_bits}, AB = {addr_bits};
        const phaseDuration = {speed_ms} / 3;
        const canvas = document.getElementById('hwCanvas');
        const ctx = canvas.getContext('2d');

        let currentStep = 0;
        let phase = 0; 
        let startTime = 0;
        let isPlaying = false;
        let isTransitioning = false;

        const CPU = {{ x: 475, y: 60, w: 850, h: 85 }};
        const PIN_DEC = 220, PIN_NORM = 400, PIN_RD = 480, PIN_WR = 520, DATA_BUS = 900;
        const DEC_X = PIN_DEC, DEC_Y = 270, DEC_W = 140, DEC_H = Math.max(90, NB * 18 + 40);

        function getChipY(b) {{ return 450 + b * ({chip_h} + {chip_gap}); }}

        function draw() {{
            ctx.clearRect(0, 0, 950, canvas.height);
            const e = logs[currentStep];
            const isConflict = e.Conflict === "YES";

            ctx.strokeStyle="#fff"; ctx.lineWidth=2;
            ctx.strokeRect(CPU.x-CPU.w/2, CPU.y-CPU.h/2, CPU.w, CPU.h);
            ctx.fillStyle="#fff"; ctx.textAlign="center"; ctx.font="bold 20px Arial";
            ctx.fillText("CPU", CPU.x, CPU.y);
            
            ctx.font="11px Arial"; ctx.fillStyle="#aaa";
            ctx.fillText("Address bus", 240, CPU.y - 25);
            ctx.fillText("{bank_label}", PIN_DEC, CPU.y + 25);
            ctx.fillText("{row_label}", PIN_NORM, CPU.y + 25);
            ctx.fillText("RD", PIN_RD, CPU.y + 25);
            ctx.fillText("WR", PIN_WR, CPU.y + 25);
            ctx.fillText("Data bus", DATA_BUS, CPU.y + 25);

            ctx.strokeStyle="#444"; ctx.lineWidth=1;
            ctx.beginPath(); ctx.moveTo(120, 145); ctx.lineTo(DATA_BUS, 145); ctx.stroke();

            ctx.strokeStyle="#fff"; ctx.lineWidth=1.5;
            ctx.strokeRect(DEC_X-DEC_W/2, DEC_Y-DEC_H/2, DEC_W, DEC_H);
            ctx.fillStyle="#fff"; ctx.font="bold 13px Arial";
            ctx.fillText("Decoder", DEC_X, DEC_Y - DEC_H/2 + 20);
            
            for(let i=0; i<NB; i++) {{
                ctx.fillStyle = (e.Bank === i && phase >= 2) ? "#00ff41" : "#777";
                ctx.font = "11px Arial";
                ctx.fillText(i, DEC_X + DEC_W/2 - 15, DEC_Y - DEC_H/2 + 40 + i*18);
            }}
            ctx.strokeStyle="#fff";
            ctx.beginPath(); ctx.moveTo(PIN_DEC, CPU.y+CPU.h/2); ctx.lineTo(PIN_DEC, DEC_Y-DEC_H/2); ctx.stroke();

            [PIN_NORM, PIN_RD, PIN_WR, DATA_BUS].forEach(x => {{
                ctx.strokeStyle = "#333";
                ctx.beginPath(); ctx.moveTo(x, CPU.y+CPU.h/2); ctx.lineTo(x, getChipY(NB-1)+40); ctx.stroke();
            }});

            const chipW = 180, chipH = {chip_h};
            for(let i=0; i<NB; i++) {{
                let cy = getChipY(i);
                let isAct = (e.Bank === i);
                
                ctx.lineWidth = isAct ? 3 : 1;
                if (isAct && isConflict && phase >= 2) {{
                    ctx.strokeStyle = "#ff1744"; 
                    ctx.fillStyle = "rgba(255,23,68,0.15)";
                }} else if (isAct) {{
                    ctx.strokeStyle = "#00ff41"; 
                    ctx.fillStyle = "rgba(0,255,65,0.08)";
                }} else {{
                    ctx.strokeStyle = "#333";
                    ctx.fillStyle = "#050505";
                }}
                
                ctx.strokeRect(680-chipW/2, cy-chipH/2, chipW, chipH);
                ctx.fillRect(680-chipW/2, cy-chipH/2, chipW, chipH);
                
                ctx.fillStyle = isAct ? "#fff" : "#777"; ctx.font="bold 12px Arial";
                ctx.fillText("BANK " + i, 680, cy - chipH/2 + 20);
                
                if(isAct) {{
                    ctx.fillStyle="#FF9800"; ctx.fillText("ROW: " + e.Row, 680, cy);
                    if(isConflict && phase >= 2) {{
                        ctx.fillStyle = "#ff1744"; ctx.font = "bold 13px Arial";
                        ctx.fillText("CONFLICT: WAIT", 680, cy + 25);
                    }}
                }}

                ctx.textAlign="left"; ctx.font="9px Arial"; ctx.fillStyle="#aaa";
                ctx.fillText("CS1", 595, cy-chipH/2 + 15);
                ctx.fillText("RD", 595, cy-5);
                ctx.fillText("WR", 595, cy+10);
                ctx.fillText("ADR", 595, cy+chipH/2 - 15);
                ctx.textAlign="center";

                [cy-5, cy+10, cy+chipH/2-15].forEach((y, idx) => {{
                    let spineX = [PIN_RD, PIN_WR, PIN_NORM][idx];
                    ctx.strokeStyle="#333"; ctx.beginPath(); ctx.moveTo(spineX, y); ctx.lineTo(590, y); ctx.stroke();
                }});

                let wireY = cy-chipH/2+12;
                let jx = DEC_X + DEC_W/2 + 10 + i*8; 
                ctx.strokeStyle = isAct ? (isConflict ? "#ff1744" : "#00ff41") : "#222";
                ctx.beginPath();
                ctx.moveTo(DEC_X+DEC_W/2, DEC_Y - DEC_H/2 + 40 + i*18);
                ctx.lineTo(jx, DEC_Y - DEC_H/2 + 40 + i*18);
                ctx.lineTo(jx, wireY);
                ctx.lineTo(590, wireY);
                ctx.stroke();

                ctx.strokeStyle = (isAct && phase === 3 && !isConflict) ? "#00ff41" : "#222";
                ctx.beginPath(); ctx.moveTo(680+chipW/2, cy); ctx.lineTo(DATA_BUS, cy); ctx.stroke();
            }}

            if(phase > 0) {{
                let t = Math.min((performance.now()-startTime)/phaseDuration, 1);
                
                if(phase === 1) {{ 
                    ctx.fillStyle = "#ffeb3b"; ctx.shadowBlur=10; ctx.shadowColor="#ffeb3b";
                    move(PIN_DEC, CPU.y+40, DEC_X, DEC_Y-DEC_H/2, t);
                }} 
                else if(phase === 2) {{
                    ctx.fillStyle = isConflict ? "#ff1744" : "#ffeb3b";
                    let wireY = getChipY(e.Bank) - chipH/2 + 12;
                    let jx = DEC_X + DEC_W/2 + 10 + e.Bank*8;
                    moveOrtho(DEC_X+DEC_W/2, DEC_Y - DEC_H/2 + 40 + e.Bank*18, 590, wireY, t, jx);
                }} 
                else if(phase === 3) {{
                    if(!isConflict) {{
                        ctx.fillStyle="#00ff41"; ctx.shadowBlur=10; ctx.shadowColor="#00ff41";
                        move(680+chipW/2, getChipY(e.Bank), DATA_BUS, getChipY(e.Bank), t);
                    }} else {{
                        ctx.fillStyle="#ff1744";
                        ctx.beginPath(); ctx.arc(680, getChipY(e.Bank), 12 * (1-t), 0, 7); ctx.fill(); 
                    }}
                }}
                
                ctx.shadowBlur=0;
                
                if(t>=1) {{
                    if(phase < 3) {{ phase++; startTime=performance.now(); }}
                    else if(isPlaying && !isTransitioning) {{ 
                        isTransitioning = true;
                        setTimeout(() => {{
                            currentStep++;
                            if (currentStep >= logs.length) {{ currentStep=0; isPlaying=false; document.getElementById('playBtn').innerText="PLAY ALL"; phase=0; }}
                            else {{ phase=1; startTime=performance.now(); }}
                            updateUI(); isTransitioning = false;
                        }}, 700);
                    }} else if (!isPlaying) {{ phase=0; }}
                }}
            }}
            requestAnimationFrame(draw);
        }}

        function move(x1,y1,x2,y2,t) {{ ctx.beginPath(); ctx.arc(x1+(x2-x1)*t, y1+(y2-y1)*t, 6, 0, 7); ctx.fill(); }}
        function moveOrtho(x1, y1, x2, y2, t, jx) {{
            if(t < 0.3) move(x1, y1, jx, y1, t/0.3);
            else if(t < 0.7) move(jx, y1, jx, y2, (t-0.3)/0.4);
            else move(jx, y2, x2, y2, (t-0.7)/0.3);
        }}

        window.togglePlay = function() {{ isPlaying=!isPlaying; document.getElementById('playBtn').innerText=isPlaying?"PAUSE":"PLAY ALL"; if(isPlaying && phase===0) {{phase=1; startTime=performance.now();}} }}
        window.nextStep = function() {{ isPlaying=false; document.getElementById('playBtn').innerText="PLAY ALL"; currentStep=(currentStep+1)%logs.length; phase=1; startTime=performance.now(); updateUI(); }}
        window.prevStep = function() {{ isPlaying=false; document.getElementById('playBtn').innerText="PLAY ALL"; currentStep=(currentStep-1+logs.length)%logs.length; phase=1; startTime=performance.now(); updateUI(); }}

        function updateUI() {{
            const e = logs[currentStep];
            document.getElementById('step-cnt').innerText = currentStep + 1;
            document.getElementById('addr-dec').innerText = e["Address(dec)"];
            document.getElementById('addr-hex').innerText = "0x" + e["Address(dec)"].toString(16).toUpperCase().padStart(4, '0');
            document.getElementById('bnk-val').innerText = e.Bank;
            document.getElementById('row-val').innerText = e.Row;
            let bin = e["Address(bin)"];
            let r = bin.slice(0, RB), b = bin.slice(RB, RB+BB), o = bin.slice(RB+BB);
            document.getElementById('bin-str').innerHTML = `<span style="color:#4fc3f7">${{r}}</span> <span style="color:#ce93d8">${{b}}</span> <span style="color:#a5d6a7">${{o}}</span>`;
        }}
        updateUI(); draw();
    </script>
    """
    components.html(html_code, height=total_h+150)

with st.sidebar:
    st.header("Simulation Parameters")
    banks = st.selectbox("Number Of Banks", [1,2,4,8,16], index=2)
    word_size = st.selectbox("Word Size (Bytes)", [1,2,4,8,16,32,64], index=2)
    capacity = st.number_input("Memory Capacity (Bytes)", value=1024, step=128)
    base_address = st.number_input("Base Address", value=0, min_value=0, max_value=capacity-1)
    latency = st.slider("Bank Access Time (Cycles)", 1, 10, 4)
    pattern = st.selectbox("Pattern", ["Sequential", "Random", "Stride"])
    
    stride_val = 1
    if pattern == "Stride":
        stride_val = st.number_input("Stride Value (in Words)", min_value=1, value=banks)
        st.info(f"Stride of {banks} words will hit the same bank repeatedly.")
        
    n_access = st.slider("Number of Accesses", 5, 50, 15)
    anim_speed = st.slider("Animation Speed", 1, 5, 3)

addr_bits = math.ceil(math.log2(capacity))
bank_bits = math.ceil(math.log2(banks))
offset_bits = math.ceil(math.log2(word_size))
row_bits = max(0, addr_bits - bank_bits - offset_bits)
rows_per_bank = capacity // (banks * word_size)

if st.sidebar.button("▶ Run Simulation"):
    addresses = generate_addresses(pattern, base_address, n_access, word_size, capacity, stride_val)
    logs, total_cyc, conflicts, stall = simulate(addresses, word_size, banks, rows_per_bank, latency, addr_bits)
    
    st.subheader("Performance Metrics")
    c1, c2, c3, c4 = st.columns(4)
    c1.metric("Total Cycles", total_cyc)
    c2.metric("Conflicts", conflicts)
    c3.metric("Stall Cycles", stall)
    c4.metric("Throughput", f"{len(logs)/total_cyc:.2f}")

    st.subheader("Address Mapping Table")
    st.dataframe(pd.DataFrame(logs)[["Access", "Address(dec)", "Address(bin)", "Bank", "Row", "StartCycle", "EndCycle", "Conflict"]], use_container_width=True)

    st.subheader("Bank Access Timeline")
    st.pyplot(plot_timeline(logs, banks))

    st.subheader("CPU-Memory Hardware Visualizer")
    animate_hardware(logs, banks, addr_bits, bank_bits, row_bits, offset_bits, word_size, rows_per_bank, anim_speed)
