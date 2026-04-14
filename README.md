# airline-ai-platform2
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI 航空服务优化平台 | AI 항공 서비스 최적화 플랫폼</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;700&family=Noto+Sans+SC:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans SC', 'Noto Sans KR', sans-serif; background-color: #f4f7f9; color: #2c3e50; }
        .navbar { background-color: #003366; box-shadow: 0 2px 10px rgba(0,0,0,0.15); }
        .lang-switch { cursor: pointer; font-weight: bold; color: #fff; border: 1px solid #fff; padding: 4px 15px; border-radius: 20px; transition: 0.3s; }
        .lang-switch:hover { background: #fff; color: #003366; }
        .card { border: none; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.06); margin-bottom: 20px; }
        .card-header { background-color: #fff; border-bottom: 1px solid #eee; font-weight: 700; color: #003366; }
        .btn-primary { background-color: #004c97; border: none; padding: 10px; font-weight: 600; }
        #output-area { display: none; }
        .script-box { background-color: #f8f9fa; border-left: 5px solid #004c97; padding: 15px; font-style: italic; color: #444; border-radius: 4px; }
        .progress { height: 22px; border-radius: 11px; }
    </style>
</head>
<body>

<nav class="navbar navbar-dark mb-4">
    <div class="container d-flex justify-content-between align-items-center">
        <span class="navbar-brand mb-0 h1" id="navTitle">AI 航空服务优化与客户体验管理平台</span>
        <div class="lang-switch" onclick="toggleLanguage()" id="langBtn">한국어</div>
    </div>
</nav>

<div class="container">
    <div class="row">
        <div class="col-md-5">
            <div class="card">
                <div class="card-header" id="inputHeader">服务决策输入</div>
                <div class="card-body">
                    <form id="decisionForm">
                        <div class="mb-3">
                            <label class="form-label" id="lblDelay">延误时间 (小时)</label>
                            <input type="number" id="delayTime" class="form-control" placeholder="例如: 3.5" step="0.1">
                        </div>
                        <div class="mb-3">
                            <label class="form-label" id="lblPType">乘客类型</label>
                            <select id="passengerType" class="form-select">
                                <option value="Normal" id="optNormal">普通旅客</option>
                                <option value="Business" id="optBusiness">商务/高端旅客</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label class="form-label" id="lblCabin">当前舱位</label>
                            <select id="cabinType" class="form-select">
                                <option value="EY" id="optEY">经济舱</option>
                                <option value="BS" id="optBS">商务舱</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label class="form-label" id="lblStaff">员工服务满意度自评 (1-5)</label>
                            <input type="range" id="staffScore" class="form-range" min="1" max="5" value="3">
                        </div>
                        <button type="button" onclick="generateDecision()" class="btn btn-primary w-100" id="btnSubmit">生成AI决策方案</button>
                    </form>
                </div>
            </div>
        </div>

        <div class="col-md-7" id="output-area">
            <div class="card border-primary">
                <div class="card-header" id="outputHeader">AI 推荐服务方案</div>
                <div class="card-body">
                    <h4 id="solutionTitle" class="text-primary mb-3"></h4>
                    <div class="script-box mb-4" id="speechScript"></div>
                    <div class="row align-items-center mb-3">
                        <div class="col-4 text-center">
                            <div class="display-6 fw-bold" id="satScore">0</div>
                            <div class="text-muted small" id="lblSatRes">满意度预测得分</div>
                        </div>
                        <div class="col-8">
                            <div class="d-flex justify-content-between mb-1">
                                <span id="lblSatLevel">预测等级</span>
                                <span id="satLevelText" class="fw-bold"></span>
                            </div>
                            <div class="progress">
                                <div id="satBar" class="progress-bar" role="progressbar"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="card bg-light">
                <div class="card-body">
                    <h6 class="text-muted" id="lblAiExp">AI 决策分析</h6>
                    <p class="small mb-0" id="aiExplanation"></p>
                </div>
            </div>
        </div>
    </div>
</div>

<footer class="text-center mt-5 text-muted pb-4">
    <small id="footerNote">© 2026 韩国航空战略开发部 - 人是目的，而非手段</small>
</footer>

<script>
let currentLang = 'zh';

const translations = {
    zh: {
        navTitle: "AI 航空服务优化与客户体验管理平台",
        langBtn: "한국어",
        inputHeader: "服务决策输入",
        lblDelay: "延误时间 (小时)",
        lblPType: "乘客类型",
        optNormal: "普通旅客",
        optBusiness: "商务/高端旅客",
        lblCabin: "当前舱位",
        optEY: "经济舱",
        optBS: "商务舱",
        lblStaff: "员工服务满意度自评 (1-5)",
        btnSubmit: "生成AI决策方案",
        outputHeader: "AI 推荐服务方案",
        lblSatRes: "满意度预测得分",
        lblSatLevel: "预测等级",
        lblAiExp: "AI 逻辑解释",
        footerNote: "© 2026 韩国航空战略开发部 - 人是目的，而非手段",
        levels: ["低", "中", "高"],
        solutions: {
            none: "常规等待与信息同步",
            meal: "发放机场餐饮券",
            rebook: "协助办理免费改签",
            vip: "优先改签 + 贵宾休息室服务",
            hotel: "酒店住宿安排",
            hotelVip: "五星级酒店安置 + 专车接送 + VIP服务"
        },
        scripts: {
            none: "“旅客您好，延误在一小时内。我们会持续更新信息，请您稍候。”",
            meal: "“抱歉让您久等，请收下这张餐券，您可以在机场餐厅享用美食。”",
            rebook: "“延误时间较长，我们正为您全力协调更早的航班改签。”",
            vip: "“尊贵的客人，我们为您安排了休息室。改签完成后我们会通知您。”",
            hotel: "“由于延误过长，我们已备好酒店，请随员工前往班车处休息。”",
            hotelVip: "“深感抱歉。我们已为您安排了特级酒店与专车，确保您的休息品质。”"
        }
    },
    ko: {
        navTitle: "AI 항공 서비스 최적화 플랫폼",
        langBtn: "中文",
        inputHeader: "서비스 의사결정 입력",
        lblDelay: "지연 시간 (시간)",
        lblPType: "승객 유형",
        optNormal: "일반 승객",
        optBusiness: "비즈니스/우수 승객",
        lblCabin: "현재 좌석",
        optEY: "이코노미석",
        optBS: "비즈니스석",
        lblStaff: "직원 서비스 만족도 자가진단 (1-5)",
        btnSubmit: "AI 의사결정 생성",
        outputHeader: "AI 추천 서비스 방안",
        lblSatRes: "만족도 예측 점수",
        lblSatLevel: "예측 등급",
        lblAiExp: "AI 의사결정 설명",
        footerNote: "© 2026 한국항공 전략개발부 - 인간은 수단이 아닌 목적입니다",
        levels: ["낮음", "중간", "높음"],
        solutions: {
            none: "정기 정보 업데이트 및 대기",
            meal: "공항 식사권/바우처 제공",
            rebook: "무료 여정 변경 지원",
            vip: "우선 리부킹 + VIP 라운지 서비스",
            hotel: "호텔 숙박 마련",
            hotelVip: "5성급 호텔 제공 + 전용 차량 + VIP 케어"
        },
        scripts: {
            none: "“승객님, 현재 지연은 1시간 이내입니다. 업데이트 상황을 지속적으로 안내해 드리겠습니다.”",
            meal: "“오래 기다리게 해드려 죄송합니다. 준비해 드린 식사권으로 공항 내 식당을 이용해 주시기 바랍니다.”",
            rebook: "“지연이 길어짐에 따라, 가장 빠른 항공편으로 변경할 수 있도록 도와드리겠습니다.”",
            vip: "“귀빈분들을 위해 라운지 서비스를 준비했습니다. 변경 사항이 생기면 바로 안내해 드리겠습니다.”",
            hotel: "“장시간 지연으로 호텔 숙박을 준비했습니다. 직원을 따라 셔틀버스로 이동해 주십시오.”",
            hotelVip: "“진심으로 사과드립니다. 최고급 호텔과 전용 차량을 준비했으니 편안히 휴식하시기 바랍니다.”"
        }
    }
};

function toggleLanguage() {
    currentLang = currentLang === 'zh' ? 'ko' : 'zh';
    updateUI();
}

function updateUI() {
    const t = translations[currentLang];
    // 更新所有文本 ID
    const ids = ['navTitle', 'langBtn', 'inputHeader', 'lblDelay', 'lblPType', 'optNormal', 'optBusiness', 'lblCabin', 'optEY', 'optBS', 'lblStaff', 'btnSubmit', 'outputHeader', 'lblSatRes', 'lblSatLevel', 'lblAiExp', 'footerNote'];
    ids.forEach(id => {
        const el = document.getElementById(id);
        if (el) el.innerText = t[id];
    });

    // 如果输出区域已显示，重新刷新计算结果以应用新语言
    if (document.getElementById('output-area').style.display === 'block') {
        generateDecision();
    }
}

function generateDecision() {
    const delayVal = document.getElementById('delayTime').value;
    if (delayVal === "") {
        alert(currentLang === 'zh' ? "请输入延误时间" : "지연 시간을 입력해 주세요");
        return;
    }
    const delay = parseFloat(delayVal);
    const pType = document.getElementById('passengerType').value;
    const cabin = document.getElementById('cabinType').value;
    const staffScore = parseInt(document.getElementById('staffScore').value);
    const t = translations[currentLang];

    let solKey = "";
    let bonus = 0;

    // 决策逻辑
    if (delay < 1) {
        solKey = "none";
    } else if (delay <= 3) {
        solKey = "meal"; bonus = 5;
    } else if (delay <= 5) {
        if (pType === "Business" || cabin === "BS") {
            solKey = "vip"; bonus = 20;
        } else {
            solKey = "rebook";
        }
    } else {
        if (pType === "Business" || cabin === "BS") {
            solKey = "hotelVip"; bonus = 35;
        } else {
            solKey = "hotel"; bonus = 15;
        }
    }

    // 满意度计算
    let staffImpact = (staffScore - 3) * 10;
    let finalScore = Math.max(0, Math.min(100, 100 - (delay * 10) + bonus + staffImpact));

    // UI 显示
    document.getElementById('output-area').style.display = 'block';
    document.getElementById('solutionTitle').innerText = t.solutions[solKey];
    document.getElementById('speechScript').innerText = t.scripts[solKey];
    document.getElementById('satScore').innerText = Math.round(finalScore);
    
    const bar = document.getElementById('satBar');
    const levelText = document.getElementById('satLevelText');
    bar.style.width = finalScore + "%";
    
    if (finalScore >= 80) {
        bar.className = "progress-bar bg-success";
        levelText.innerText = t.levels[2];
        levelText.className = "fw-bold text-success";
    } else if (finalScore >= 50) {
        bar.className = "progress-bar bg-warning";
        levelText.innerText = t.levels[1];
        levelText.className = "fw-bold text-warning";
    } else {
        bar.className = "progress-bar bg-danger";
        levelText.innerText = t.levels[0];
        levelText.className = "fw-bold text-danger";
    }

    // AI 解释
    const expText = currentLang === 'zh' 
        ? `针对延误${delay}小时的${pType === 'Business' ? '商务' : '普通'}旅客，系统推荐“${t.solutions[solKey]}”。满意度计算：延误扣分(${delay*10}) + 补偿得分(${bonus}) + 员工表现(${staffImpact}) = ${Math.round(finalScore)}。`
        : `${delay}시간 지연된 ${pType === 'Business' ? '비즈니스' : '일반'} 승객에 대해 "${t.solutions[solKey]}"을(를) 권장합니다. 만족도 산출: 지연 감점(${delay*10}) + 보상 가점(${bonus}) + 직원 성과(${staffImpact}) = ${Math.round(finalScore)}점입니다.`;
    document.getElementById('aiExplanation').innerText = expText;
}
</script>
</body>
</html>
