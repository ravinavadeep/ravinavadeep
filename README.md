import random, os
random.seed(7)
os.makedirs("assets", exist_ok=True)

def waves(y, h, fill, op, dur, amp):
    d = f"M0 {amp} Q150 0 300 {amp} T600 {amp}"
    for x in range(900, 2500, 300):
        d += f" T{x} {amp}"
    d += f" V{h} H0 Z"
    return f'''<g transform="translate(0,{y})"><path d="{d}" fill="{fill}" fill-opacity="{op}">
<animateTransform attributeName="transform" type="translate" from="0 0" to="-600 0" dur="{dur}s" repeatCount="indefinite"/></path></g>'''

def grad(id_, cols, dur=14):
    n = len(cols)
    stops = ""
    for i in range(n):
        vals = ";".join(cols[(i+k) % n] for k in range(n)) + ";" + cols[i]
        stops += f'<stop offset="{i/(n-1):.2f}" stop-color="{cols[i]}"><animate attributeName="stop-color" values="{vals}" dur="{dur}s" repeatCount="indefinite"/></stop>'
    return f'<linearGradient id="{id_}" x1="0" y1="0" x2="1" y2="1">{stops}</linearGradient>'

# ---------- HERO ----------
W, H = 1200, 340
bubbles = ""
for i in range(34):
    x = random.randint(0, W); r = random.uniform(2, 9)
    y0 = random.randint(200, H); dur = random.uniform(6, 14); dl = random.uniform(0, 8)
    bubbles += f'<circle cx="{x}" cy="{y0}" r="{r:.1f}" fill="#fff" fill-opacity="0.25"><animate attributeName="cy" from="{H+20}" to="-20" dur="{dur:.1f}s" begin="-{dl:.1f}s" repeatCount="indefinite"/><animate attributeName="opacity" values="0;0.9;0" dur="{dur:.1f}s" begin="-{dl:.1f}s" repeatCount="indefinite"/></circle>'
stars = ""
for i in range(40):
    x = random.randint(0, W); y = random.randint(0, 220); d = random.uniform(1.5, 4)
    stars += f'<circle cx="{x}" cy="{y}" r="1.4" fill="#fff"><animate attributeName="opacity" values="0.1;1;0.1" dur="{d:.1f}s" begin="-{random.uniform(0,4):.1f}s" repeatCount="indefinite"/></circle>'

hero = f'''<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {W} {H}" width="100%">
<defs>
{grad("bg", ["#1e3c72","#6a11cb","#ff0080","#00c6ff"], 16)}
<filter id="glow" x="-20%" y="-50%" width="140%" height="200%"><feGaussianBlur stdDeviation="5" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
<clipPath id="r"><rect width="{W}" height="{H}" rx="24"/></clipPath>
</defs>
<g clip-path="url(#r)">
<rect width="{W}" height="{H}" fill="url(#bg)"/>
{stars}
{bubbles}
<circle cx="980" cy="90" r="120" fill="#fff" fill-opacity="0.06"><animate attributeName="r" values="110;140;110" dur="7s" repeatCount="indefinite"/></circle>
<circle cx="180" cy="230" r="90" fill="#fff" fill-opacity="0.05"><animate attributeName="r" values="80;110;80" dur="9s" repeatCount="indefinite"/></circle>
{waves(255, 120, "#ffffff", 0.12, 9, 30)}
{waves(275, 120, "#ffffff", 0.18, 6, 24)}
{waves(295, 120, "#0b1026", 0.55, 11, 20)}
<g font-family="'Segoe UI', Arial, Helvetica, sans-serif" text-anchor="middle" fill="#fff">
<text x="600" y="150" font-size="76" font-weight="800" filter="url(#glow)" letter-spacing="2">Ravi Navadeep
<animate attributeName="opacity" from="0" to="1" dur="1.6s" fill="freeze"/>
<animateTransform attributeName="transform" type="translate" from="0 30" to="0 0" dur="1.2s" fill="freeze"/></text>
<text x="600" y="205" font-size="24" fill-opacity="0.95">Computer Science Student  •  Frontend Developer  •  AI Enthusiast
<animate attributeName="opacity" from="0" to="1" begin="1s" dur="1.5s" fill="freeze"/></text>
<text x="600" y="240" font-size="18" fill-opacity="0.8">📍 Tanuku, India
<animate attributeName="opacity" from="0" to="0.85" begin="1.8s" dur="1.5s" fill="freeze"/></text>
</g>
</g></svg>'''
open("assets/hero.svg", "w", encoding="utf-8").write(hero)

# ---------- SECTION BANNERS ----------
sections = [
 ("about", "👨‍💻  About Me", ["#11998e","#38ef7d","#2193b0"]),
 ("stack", "🛠️  Tech Stack", ["#fc4a1a","#f7b733","#ff512f"]),
 ("projects", "🚀  Featured Projects", ["#4776e6","#8e54e9","#00c6ff"]),
 ("stats", "📊  GitHub Stats", ["#f953c6","#b91d73","#7f00ff"]),
 ("achieve", "🏆  Achievements &amp; Milestones", ["#f7971e","#ffd200","#ff5e62"]),
 ("learning", "📚  Currently Learning", ["#00c6ff","#0072ff","#7b2ff7"]),
 ("collab", "🤝  Open to Collaborate On", ["#43cea2","#185a9d","#00c9ff"]),
 ("connect", "📫  Connect With Me", ["#ee0979","#ff6a00","#7b2ff7"]),
]
for key, title, cols in sections:
    w, h = 1200, 90
    svg = f'''<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {w} {h}" width="100%">
<defs>
{grad("g", cols, 8)}
<linearGradient id="s" x1="0" y1="0" x2="1" y2="0"><stop offset="0" stop-color="#fff" stop-opacity="0"/><stop offset="0.5" stop-color="#fff" stop-opacity="0.45"/><stop offset="1" stop-color="#fff" stop-opacity="0"/></linearGradient>
<clipPath id="c"><rect width="{w}" height="{h}" rx="18"/></clipPath>
</defs>
<g clip-path="url(#c)">
<rect width="{w}" height="{h}" fill="url(#g)"/>
<rect y="0" width="220" height="{h}" fill="url(#s)" transform="skewX(-20)"><animate attributeName="x" from="-400" to="1500" dur="3.8s" repeatCount="indefinite"/></rect>
{waves(60, 60, "#ffffff", 0.14, 7, 12)}
<text x="600" y="57" text-anchor="middle" font-family="'Segoe UI', Arial, sans-serif" font-size="34" font-weight="700" fill="#fff">{title}
<animate attributeName="opacity" values="0.85;1;0.85" dur="3s" repeatCount="indefinite"/></text>
</g></svg>'''
    open(f"assets/{key}.svg", "w", encoding="utf-8").write(svg)

# ---------- FOOTER ----------
fw, fh = 1200, 160
footer = f'''<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {fw} {fh}" width="100%">
<defs>{grad("f", ["#1e3c72","#6a11cb","#ff0080","#00c6ff"], 16)}
<clipPath id="c"><rect width="{fw}" height="{fh}" rx="24"/></clipPath></defs>
<g clip-path="url(#c)">
<rect width="{fw}" height="{fh}" fill="url(#f)"/>
{waves(40, 140, "#ffffff", 0.14, 10, 26)}
{waves(70, 140, "#ffffff", 0.2, 7, 22)}
{waves(100, 140, "#0b1026", 0.5, 12, 18)}
<text x="600" y="62" text-anchor="middle" font-family="'Segoe UI', Arial, sans-serif" font-size="26" font-weight="600" fill="#fff">Thanks for visiting ✨  Let's build something great together
<animate attributeName="opacity" values="0.7;1;0.7" dur="4s" repeatCount="indefinite"/></text>
</g></svg>'''
open("assets/footer.svg", "w", encoding="utf-8").write(footer)
print("Done! SVGs created in ./assets")
