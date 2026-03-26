<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ninebot Service Dashboard</title>
    <style>
        :root {
            --bg1: #09111d;
            --bg2: #13233a;
            --bg3: #1b3559;
            --line: rgba(255,255,255,.08);
            --text: #edf4ff;
            --muted: #9fb2d2;
            --blue: #5d8fff;
            --cyan: #59dcff;
            --green: #46df9f;
            --amber: #ffc567;
            --shadow: 0 26px 70px rgba(0,0,0,.35);
            --radius: 26px;
        }

        * {
            box-sizing: border-box;
        }

        html, body {
            margin: 0;
            min-height: 100%;
        }

        body {
            font-family: Inter, "Segoe UI", Arial, sans-serif;
            color: var(--text);
            background: radial-gradient(circle at 15% 12%, rgba(93,143,255,.24), transparent 22%), radial-gradient(circle at 85% 8%, rgba(89,220,255,.12), transparent 18%), linear-gradient(145deg, var(--bg1), var(--bg2) 55%, var(--bg3));
            overflow-x: hidden;
        }

            body::before {
                content: "";
                position: fixed;
                inset: 0;
                background-image: linear-gradient(rgba(255,255,255,.025) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,.025) 1px, transparent 1px);
                background-size: 40px 40px;
                mask-image: radial-gradient(circle at center, black 35%, transparent 86%);
                pointer-events: none;
                opacity: .35;
            }

        .app {
            max-width: 1520px;
            margin: 0 auto;
            padding: 22px;
            perspective: 1800px;
        }

        .topbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 16px;
            margin-bottom: 20px;
        }

        .brand {
            display: flex;
            gap: 16px;
            align-items: center;
        }

        .brand-badge {
            width: 74px;
            height: 74px;
            border-radius: 24px;
            display: grid;
            place-items: center;
            font-size: 30px;
            background: linear-gradient(145deg, #5d8fff, #315fd8);
            box-shadow: 0 0 28px rgba(93,143,255,.26), inset 0 1px 0 rgba(255,255,255,.22);
        }

        .brand h1 {
            margin: 0;
            font-size: 32px;
            font-weight: 900;
        }

        .brand p {
            margin: 6px 0 0;
            color: var(--muted);
        }

        .system-chip {
            padding: 12px 16px;
            border-radius: 999px;
            font-weight: 800;
            background: rgba(70,223,159,.12);
            border: 1px solid rgba(70,223,159,.26);
            color: #b0f7d7;
            white-space: nowrap;
        }

        .glass {
            background: linear-gradient(180deg, rgba(10,18,30,.86), rgba(17,29,49,.78));
            border: 1px solid var(--line);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            backdrop-filter: blur(18px);
            overflow: hidden;
        }

        .view-stack {
            position: relative;
            min-height: 1040px;
        }

        .view {
            width: 100%;
            transition: opacity .6s ease, transform .8s cubic-bezier(.2,.8,.2,1), filter .6s ease;
            transform-style: preserve-3d;
        }

            .view.hidden {
                display: none;
            }

        .hidden {
            display: none !important;
        }

        .dashboard-view.enter {
            animation: dashboardIn .8s cubic-bezier(.2,.8,.2,1) both;
        }

        .dashboard-view.exit {
            animation: dashboardOut .65s cubic-bezier(.2,.8,.2,1) both;
        }

        .content-view.enter {
            animation: contentIn .8s cubic-bezier(.2,.8,.2,1) both;
        }

        .content-view.exit {
            animation: contentOut .55s cubic-bezier(.2,.8,.2,1) both;
        }

        @keyframes dashboardOut {
            0% {
                opacity: 1;
                transform: translateX(0) rotateY(0deg) scale(1);
                filter: blur(0);
            }

            100% {
                opacity: 0;
                transform: translateX(-140px) rotateY(12deg) scale(.94);
                filter: blur(12px);
            }
        }

        @keyframes contentIn {
            0% {
                opacity: 0;
                transform: translateX(140px) rotateY(-12deg) scale(.94);
                filter: blur(12px);
            }

            100% {
                opacity: 1;
                transform: translateX(0) rotateY(0deg) scale(1);
                filter: blur(0);
            }
        }

        @keyframes contentOut {
            0% {
                opacity: 1;
                transform: translateX(0) rotateY(0deg) scale(1);
                filter: blur(0);
            }

            100% {
                opacity: 0;
                transform: translateX(140px) rotateY(-10deg) scale(.96);
                filter: blur(10px);
            }
        }

        @keyframes dashboardIn {
            0% {
                opacity: 0;
                transform: translateX(-120px) rotateY(10deg) scale(.95);
                filter: blur(10px);
            }

            100% {
                opacity: 1;
                transform: translateX(0) rotateY(0deg) scale(1);
                filter: blur(0);
            }
        }

        .hero, .content {
            padding: 24px;
        }

        .eyebrow {
            color: #a7c6ff;
            text-transform: uppercase;
            letter-spacing: 1.6px;
            font-size: 12px;
            font-weight: 800;
            margin-bottom: 8px;
        }

        h2, h3 {
            margin: 0;
            font-weight: 900;
        }

        .lead, .content-head p {
            margin: 10px 0 0;
            color: var(--muted);
            line-height: 1.75;
        }

        .dashboard-wrap {
            margin-top: 22px;
            min-height: 930px;
            border-radius: 30px;
            border: 1px solid rgba(255,255,255,.07);
            position: relative;
            overflow: hidden;
            background: radial-gradient(circle at center bottom, rgba(93,143,255,.2), transparent 30%), linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.01)), #0a1322;
        }

        .floor {
            position: absolute;
            left: 10%;
            right: 10%;
            bottom: 80px;
            height: 26px;
            border-radius: 999px;
            background: radial-gradient(circle at center, rgba(93,143,255,.22), rgba(93,143,255,.08) 46%, transparent 72%);
            filter: blur(8px);
            pointer-events: none;
        }

        .cockpit {
            position: absolute;
            inset: 0;
            display: grid;
            place-items: center;
            transition: transform .8s cubic-bezier(.2,.8,.2,1);
        }

            .cockpit.motion {
                transform: scale(1.03) translateY(-6px);
            }

        .handlebar {
            position: relative;
            width: 1120px;
            max-width: 97%;
            height: 760px;
            transform: perspective(1500px) rotateX(4deg) rotateY(-7deg);
            animation: floaty 6s ease-in-out infinite;
        }

        @keyframes floaty {
            0%,100% {
                transform: perspective(1500px) rotateX(4deg) rotateY(-7deg) translateY(0);
            }

            50% {
                transform: perspective(1500px) rotateX(5.5deg) rotateY(-4deg) translateY(-10px);
            }
        }

        .shadow {
            position: absolute;
            left: 23%;
            right: 23%;
            bottom: 70px;
            height: 50px;
            border-radius: 50%;
            background: radial-gradient(circle at center, rgba(0,0,0,.42), rgba(0,0,0,.12) 55%, transparent 78%);
            filter: blur(8px);
        }

        .bar {
            position: absolute;
            top: 164px;
            left: 50%;
            width: 860px;
            height: 38px;
            transform: translateX(-50%);
            border-radius: 18px;
            background: linear-gradient(180deg, #111827, #03070d);
            box-shadow: 0 16px 26px rgba(0,0,0,.34), inset 0 1px 0 rgba(255,255,255,.05);
        }

        .grip {
            position: absolute;
            top: 150px;
            width: 190px;
            height: 66px;
            border-radius: 34px;
            background: linear-gradient(180deg, #0f1520, #02050a);
            box-shadow: 0 14px 24px rgba(0,0,0,.3);
        }

        .grip-left {
            left: 70px;
        }

        .grip-right {
            right: 70px;
        }

        .brake {
            position: absolute;
            top: 182px;
            right: 188px;
            width: 58px;
            height: 132px;
            border: 6px solid rgba(255,255,255,.18);
            border-top: none;
            border-left-width: 9px;
            border-radius: 0 0 26px 26px;
            transform: rotate(9deg);
            opacity: .92;
        }

        .stem {
            position: absolute;
            top: 194px;
            left: 50%;
            width: 82px;
            height: 410px;
            transform: translateX(-50%);
            border-radius: 40px;
            background: linear-gradient(180deg, #c5ced8 0%, #eef2f6 18%, #c6ced8 54%, #8f99a7 100%);
            box-shadow: 0 18px 28px rgba(0,0,0,.2);
        }

        .dashboard {
            position: absolute;
            top: 76px;
            left: 50%;
            width: 182px;
            height: 660px;
            transform: translateX(-50%);
            border-radius: 40px;
            background: linear-gradient(180deg, #0d131d, #000);
            box-shadow: 0 20px 40px rgba(0,0,0,.34), 0 0 26px rgba(93,143,255,.08);
            z-index: 10;
            overflow: hidden;
        }

        .dashboard-top {
            position: absolute;
            top: 22px;
            left: 50%;
            transform: translateX(-50%);
            width: 84px;
            height: 36px;
            border-radius: 12px;
            background: #1a1f29;
        }

        .screen {
            position: absolute;
            top: 72px;
            left: 14px;
            right: 14px;
            bottom: 158px;
            border-radius: 24px;
            background: radial-gradient(circle at top, #163d77, #07111d 60%, #02060a);
            color: #cfe2ff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            padding: 14px 12px 12px;
            text-align: center;
            box-shadow: inset 0 0 0 1px rgba(255,255,255,.04);
            overflow: hidden;
        }

        .screen-label {
            font-size: 11px;
            font-weight: 800;
            letter-spacing: 1px;
            text-transform: uppercase;
            color: #96bcff;
            margin-bottom: 8px;
        }

        .screen-number {
            font-size: 64px;
            font-weight: 900;
            line-height: 1;
            margin-bottom: 8px;
            color: #b9d1ff;
            text-shadow: 0 0 16px rgba(93,143,255,.18);
            min-height: 64px;
        }

        .screen-mode {
            font-size: 18px;
            font-weight: 900;
            color: #63e6a3;
            min-height: 22px;
        }

        .screen-sub {
            margin-top: 8px;
            font-size: 12px;
            color: #dce8ff;
            min-height: 16px;
            margin-bottom: 12px;
        }

        .power-area {
            position: absolute;
            left: 50%;
            bottom: 30px;
            transform: translateX(-50%);
            z-index: 30;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            width: 120px;
        }

        .power-button {
            width: 82px;
            height: 82px;
            border-radius: 50%;
            border: 2px solid rgba(255,255,255,.12);
            background: radial-gradient(circle at 35% 35%, #2f2f2f, #111);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 14px 26px rgba(0,0,0,.32);
            transition: transform .2s ease, box-shadow .2s ease;
        }

            .power-button:hover {
                transform: scale(1.07);
            }

            .power-button.on {
                box-shadow: 0 0 24px rgba(70,223,159,.22), 0 14px 26px rgba(0,0,0,.32);
            }

        .power-symbol {
            width: 30px;
            height: 30px;
            border: 4px solid #9dc2ff;
            border-top-color: transparent;
            border-radius: 50%;
            position: relative;
        }

            .power-symbol::before {
                content: "";
                position: absolute;
                top: -14px;
                left: 50%;
                transform: translateX(-50%);
                width: 4px;
                height: 16px;
                background: #9dc2ff;
                border-radius: 4px;
            }

        .power-button.on .power-symbol {
            border-color: #49d98e;
            border-top-color: transparent;
        }

            .power-button.on .power-symbol::before {
                background: #49d98e;
            }

        .power-text {
            padding: 8px 10px;
            border-radius: 999px;
            background: rgba(9,16,28,.78);
            border: 1px solid rgba(255,255,255,.08);
            font-size: 12px;
            font-weight: 800;
            color: #dce8ff;
            white-space: nowrap;
        }

        .boot {
            position: absolute;
            top: 110px;
            left: 50%;
            transform: translateX(-50%);
            width: 260px;
            padding: 18px;
            border-radius: 24px;
            background: rgba(9,16,28,.92);
            border: 1px solid rgba(255,255,255,.08);
            box-shadow: 0 18px 36px rgba(0,0,0,.28);
            text-align: center;
            z-index: 40;
            opacity: 0;
            pointer-events: none;
            transition: opacity .28s ease;
        }

            .boot.show {
                opacity: 1;
            }

        .boot-ring {
            width: 76px;
            height: 76px;
            margin: 0 auto 14px;
            border-radius: 50%;
            border: 3px solid rgba(93,143,255,.16);
            border-top-color: #a7c7ff;
            animation: spin 1.1s linear infinite;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        .boot-title {
            color: #dce8ff;
            font-size: 12px;
            font-weight: 800;
            letter-spacing: 1.4px;
            text-transform: uppercase;
            margin-bottom: 12px;
        }

        .boot-progress {
            height: 10px;
            border-radius: 999px;
            background: rgba(255,255,255,.06);
            overflow: hidden;
        }

        .boot-fill {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #5d8fff, #59dcff);
            transition: width .18s linear;
        }

        .stage-panel {
            position: absolute;
            left: 40px;
            right: 40px;
            bottom: 34px;
            padding: 20px;
            border-radius: 24px;
            background: linear-gradient(180deg, rgba(10,18,30,.86), rgba(17,29,49,.76));
            border: 1px solid rgba(255,255,255,.08);
            box-shadow: 0 18px 36px rgba(0,0,0,.28);
            min-height: 280px;
            z-index: 5;
            transform-style: preserve-3d;
            transition: transform .55s cubic-bezier(.2,.8,.2,1), opacity .45s ease;
        }

            .stage-panel.swap {
                transform: translateY(10px) rotateX(10deg) scale(.98);
                opacity: .35;
            }

        .stage-head,
        .content-head,
        .content-topbar {
            display: flex;
            justify-content: space-between;
            align-items: start;
            gap: 14px;
            margin-bottom: 18px;
        }

        .stage-title {
            font-size: 28px;
            font-weight: 900;
            margin: 0 0 6px;
        }

        .stage-text {
            color: var(--muted);
            line-height: 1.7;
            margin: 0;
        }

        .stage-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 18px;
        }

        .ghost-btn,
        .stage-btn,
        .back-btn,
        .tab-btn,
        .parts-main-btn,
        .big-doc-btn,
        .link-btn,
        .supplier-btn,
        .mini-btn {
            border: 1px solid rgba(255,255,255,.1);
            border-radius: 14px;
            padding: 12px 16px;
            background: linear-gradient(180deg, rgba(255,255,255,.07), rgba(255,255,255,.03));
            color: var(--text);
            font-weight: 800;
            cursor: pointer;
            transition: .2s;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

            .ghost-btn:hover,
            .stage-btn:hover,
            .back-btn:hover,
            .tab-btn:hover,
            .parts-main-btn:hover,
            .big-doc-btn:hover,
            .link-btn:hover,
            .supplier-btn:hover,
            .mini-btn:hover {
                transform: translateY(-2px);
                border-color: rgba(93,143,255,.35);
            }

        .stage-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 14px;
        }

        .model-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 14px;
        }

        .brand-card,
        .model-card,
        .info-card,
        .choice-big {
            border-radius: 20px;
            border: 1px solid rgba(255,255,255,.08);
            background: linear-gradient(180deg, rgba(255,255,255,.06), rgba(255,255,255,.02));
            padding: 16px;
            cursor: pointer;
            transition: .24s;
            transform-style: preserve-3d;
        }

            .brand-card:hover,
            .model-card:hover,
            .choice-big:hover {
                transform: translateY(-4px) rotateX(4deg);
                border-color: rgba(93,143,255,.28);
            }

        .brand-logo {
            width: 92px;
            height: 92px;
            border-radius: 20px;
            display: grid;
            place-items: center;
            background: rgba(255,255,255,.04);
            border: 1px solid rgba(255,255,255,.06);
            overflow: hidden;
            margin-bottom: 14px;
            padding: 10px;
        }

            .brand-logo img {
                width: 100%;
                height: 100%;
                object-fit: contain;
                display: block;
            }

        .brand-name {
            font-weight: 900;
            font-size: 18px;
            margin-bottom: 6px;
        }

        .brand-sub {
            color: var(--muted);
            font-size: 14px;
            line-height: 1.5;
        }

        .model-card {
            padding: 14px;
        }

        .model-thumb {
            min-height: 120px;
            border-radius: 16px;
            background: linear-gradient(180deg, rgba(93,143,255,.14), rgba(255,255,255,.03));
            border: 1px solid rgba(255,255,255,.05);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 18px;
            text-align: center;
        }

        .model-thumb-text {
            font-size: 24px;
            font-weight: 900;
            line-height: 1.25;
            color: #dce8ff;
        }

        .choice-grid,
        .grid,
        .resource-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 14px;
        }

        .parts-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 14px;
            margin-bottom: 18px;
            align-items: stretch;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 14px;
            margin-bottom: 18px;
        }

        .tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 18px;
            margin-bottom: 26px;
        }

        .content-topbar {
            align-items: center;
        }

        .hero-card,
        .stat,
        .card,
        .link-group,
        .part-card,
        .wide,
        .big-center-card {
            background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.02));
            border: 1px solid rgba(255,255,255,.07);
            border-radius: 18px;
            padding: 18px;
            animation: riseIn .55s ease both;
        }

        @keyframes riseIn {
            0% {
                opacity: 0;
                transform: translateY(16px);
            }

            100% {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .hero-card {
            display: grid;
            grid-template-columns: 220px 1fr;
            gap: 18px;
            padding: 22px;
            border-radius: 24px;
            background: linear-gradient(135deg, rgba(93,143,255,.2), rgba(18,30,49,.76) 65%);
            margin-bottom: 18px;
        }

        .hero-image {
            background: rgba(255,255,255,.04);
            border: 1px solid rgba(255,255,255,.06);
            border-radius: 20px;
            overflow: hidden;
            aspect-ratio: 1 / 1;
            display: grid;
            place-items: center;
            padding: 12px;
        }

            .hero-image img {
                width: 100%;
                height: 100%;
                object-fit: contain;
                display: block;
                filter: drop-shadow(0 16px 20px rgba(0,0,0,.35));
            }

        .hero-copy h4 {
            margin: 0 0 10px;
            font-size: 30px;
        }

        .hero-copy p,
        .card p,
        .wide p,
        .wide li,
        .link-group p,
        .part-description,
        .big-center-card p {
            margin: 0;
            color: #d8e4fb;
            line-height: 1.78;
        }

        .k {
            color: #9fc2ff;
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 1.4px;
            font-weight: 800;
            margin-bottom: 8px;
        }

        .v {
            font-size: 18px;
            font-weight: 900;
            line-height: 1.55;
        }

        .card h5,
        .link-group h5,
        .wide h5,
        .big-center-card h5 {
            margin: 0 0 12px;
            font-size: 18px;
            font-weight: 900;
        }

        .wide ul {
            margin: 0;
            padding-left: 20px;
        }

        .links-row,
        .suppliers,
        .parts-main-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 28px;
            align-items: center;
        }

        .section-top-block {
            margin-bottom: 28px;
        }

        .tab-btn.active {
            background: linear-gradient(180deg, rgba(93,143,255,.28), rgba(49,95,216,.18));
            border-color: rgba(255,255,255,.16);
        }

        .parts-main-btn {
            background: linear-gradient(180deg, rgba(93,143,255,.22), rgba(49,95,216,.14));
            min-height: 46px;
        }

        .tag {
            padding: 6px 10px;
            border-radius: 999px;
            font-size: 11px;
            font-weight: 800;
            white-space: nowrap;
            background: rgba(255,197,103,.12);
            color: #ffd08a;
            border: 1px solid rgba(255,197,103,.18);
        }

        .part-top {
            display: flex;
            justify-content: space-between;
            gap: 10px;
            align-items: start;
            margin-bottom: 10px;
        }

        .part-name {
            font-weight: 900;
            font-size: 16px;
        }

        .big-doc-btn {
            background: linear-gradient(135deg, #6fa0ff, #3c68e0);
            box-shadow: 0 16px 28px rgba(55,97,230,.26);
        }

        .mini-btn.test-btn {
            background: linear-gradient(180deg, rgba(70,223,159,.18), rgba(70,223,159,.08));
            border-color: rgba(70,223,159,.24);
        }

        .mini-btn.disabled {
            opacity: .45;
            cursor: not-allowed;
            pointer-events: none;
        }

        .part-card {
            min-height: 300px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .empty {
            min-height: 420px;
            display: grid;
            place-items: center;
            text-align: center;
            color: var(--muted);
            border: 1px dashed rgba(255,255,255,.1);
            border-radius: 20px;
            background: rgba(255,255,255,.03);
            padding: 30px;
        }

        .part-modal {
            position: fixed;
            inset: 0;
            background: rgba(2,8,18,.72);
            backdrop-filter: blur(10px);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 22px;
            z-index: 9999;
            opacity: 0;
            pointer-events: none;
            transition: opacity .3s ease;
        }

            .part-modal.show {
                opacity: 1;
                pointer-events: auto;
            }

        .part-modal-card {
            width: min(760px, 100%);
            border-radius: 28px;
            border: 1px solid rgba(255,255,255,.09);
            background: linear-gradient(180deg, rgba(15,25,42,.96), rgba(13,28,52,.94));
            box-shadow: 0 30px 80px rgba(0,0,0,.42);
            padding: 26px;
            position: relative;
            transform: perspective(1400px) rotateX(16deg) scale(.8);
            opacity: 0;
            transition: transform .42s cubic-bezier(.2,.8,.2,1), opacity .32s ease;
        }

        .part-modal.show .part-modal-card {
            transform: perspective(1400px) rotateX(0deg) scale(1);
            opacity: 1;
        }

        .part-close {
            position: absolute;
            top: 14px;
            right: 14px;
            width: 42px;
            height: 42px;
            border: 1px solid rgba(255,255,255,.09);
            border-radius: 50%;
            background: rgba(255,255,255,.06);
            color: white;
            font-size: 22px;
            font-weight: 900;
            cursor: pointer;
            display: grid;
            place-items: center;
            transition: .2s;
        }

            .part-close:hover {
                transform: scale(1.08);
                background: rgba(255,255,255,.11);
            }

        .part-modal-title {
            font-size: 28px;
            font-weight: 900;
            margin: 0 0 10px;
        }

        .part-modal-sub {
            color: #cddcf8;
            line-height: 1.8;
            margin: 0 0 20px;
        }

        .part-modal-visual {
            min-height: 340px;
            border-radius: 24px;
            background: radial-gradient(circle at center, rgba(93,143,255,.20), rgba(255,255,255,.02) 60%), rgba(255,255,255,.03);
            border: 1px solid rgba(255,255,255,.06);
            display: grid;
            place-items: center;
            overflow: hidden;
            padding: 18px;
        }

            .part-modal-visual img {
                width: min(90%, 480px);
                max-height: 320px;
                object-fit: contain;
                display: block;
                filter: drop-shadow(0 26px 30px rgba(0,0,0,.35));
                animation: partFloat 4s ease-in-out infinite;
            }

        @keyframes partFloat {
            0%,100% {
                transform: translateY(0) scale(1);
            }

            50% {
                transform: translateY(-10px) scale(1.02);
            }
        }

        .error-image-modal {
            position: fixed;
            inset: 0;
            background: rgba(2, 8, 18, 0.88);
            backdrop-filter: blur(10px);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 24px;
            z-index: 99999;
            opacity: 0;
            pointer-events: none;
            transition: opacity .28s ease;
        }

            .error-image-modal.show {
                opacity: 1;
                pointer-events: auto;
            }

            .error-image-modal img {
                max-width: 96vw;
                max-height: 92vh;
                width: auto;
                height: auto;
                object-fit: contain;
                display: block;
                border-radius: 18px;
                box-shadow: 0 30px 80px rgba(0,0,0,.45);
                transform: perspective(1400px) rotateX(10deg) scale(.92);
                opacity: 0;
                transition: transform .35s cubic-bezier(.2,.8,.2,1), opacity .25s ease;
            }

            .error-image-modal.show img {
                transform: perspective(1400px) rotateX(0deg) scale(1);
                opacity: 1;
            }

        .error-image-close {
            position: fixed;
            top: 18px;
            right: 18px;
            width: 52px;
            height: 52px;
            border-radius: 50%;
            border: 1px solid rgba(255,255,255,.14);
            background: rgba(255,255,255,.08);
            color: white;
            font-size: 28px;
            font-weight: 900;
            cursor: pointer;
            z-index: 100000;
            display: grid;
            place-items: center;
            transition: .2s;
        }

            .error-image-close:hover {
                transform: scale(1.08);
                background: rgba(255,255,255,.14);
            }

        .ai-select {
            width: 100%;
            background: rgba(255,255,255,.03);
            border: 1px solid rgba(255,255,255,.08);
            border-radius: 14px;
            padding: 12px;
            color: white;
            outline: none;
        }

            .ai-select option {
                color: black;
                background: white;
            }

        @media (max-width: 1280px) {
            .model-grid {
                grid-template-columns: repeat(3, 1fr);
            }
        }

        @media (max-width: 1100px) {
            .stats, .grid, .resource-grid, .parts-grid, .stage-grid, .choice-grid, .model-grid {
                grid-template-columns: 1fr;
            }

            .dashboard {
                width: 170px;
                height: 660px;
            }

            .bar {
                width: 680px;
            }

            .grip {
                width: 150px;
            }

            .grip-left {
                left: 28px;
            }

            .grip-right {
                right: 28px;
            }

            .brake {
                right: 96px;
            }

            .content-topbar,
            .content-head,
            .topbar,
            .stage-head {
                flex-direction: column;
                align-items: flex-start;
            }

            .hero-card {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="app">
        <div class="topbar">
            <div class="brand">
                <div class="brand-badge">🛴</div>
                <div>
                    <h1>Ninebot Service Dashboard</h1>
                    <p>Micromobiliteit project • merken, modellen en identificatieflow</p>
                </div>
            </div>
            <div class="system-chip" id="systemState">Stand-by</div>
        </div>

        <div class="view-stack">
            <section class="glass hero view dashboard-view" id="dashboardView">
                <div class="eyebrow">Dashboard / stuur</div>
                <h2>Druk op de powerknop op de display</h2>
                <p class="lead">Na het opstarten verschijnen eerst de merken. Daarna kan je verder naar modellen of naar identificatie.</p>

                <div class="dashboard-wrap">
                    <div class="floor"></div>

                    <div class="cockpit" id="cockpit">
                        <div class="handlebar">
                            <div class="shadow"></div>
                            <div class="bar"></div>
                            <div class="grip grip-left"></div>
                            <div class="grip grip-right"></div>
                            <div class="brake"></div>
                            <div class="stem"></div>

                            <div class="dashboard">
                                <div class="dashboard-top"></div>

                                <div class="screen">
                                    <div class="screen-label" id="screenLabel">Display</div>
                                    <div class="screen-number" id="screenNumber">0</div>
                                    <div class="screen-mode" id="screenMode">OFF</div>
                                    <div class="screen-sub" id="screenSub">Druk op power</div>
                                </div>

                                <div class="power-area">
                                    <button class="power-button" id="powerButton" aria-label="Power knop" type="button">
                                        <div class="power-symbol"></div>
                                    </button>
                                    <div class="power-text">Power</div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="boot" id="bootBox">
                        <div class="boot-ring"></div>
                        <div class="boot-title">Dashboard booting</div>
                        <div class="boot-progress">
                            <div class="boot-fill" id="bootFill"></div>
                        </div>
                    </div>

                    <div class="stage-panel hidden" id="stagePanel"></div>
                </div>
            </section>

            <section class="glass content view content-view hidden" id="contentView">
                <div class="content-topbar">
                    <button class="back-btn" id="backToDashboard">← Terug naar dashboard</button>
                    <div class="system-chip" id="modelStatus">Offline</div>
                </div>

                <div class="content-head">
                    <div>
                        <div class="eyebrow">Model inhoud</div>
                        <h3 id="titleModel">Systeem uitgeschakeld</h3>
                        <p id="titleDesc">Zet eerst de step aan. Daarna kan je via het dashboard door de merken en modellen gaan.</p>
                    </div>
                </div>

                <div class="tabs">
                    <button class="tab-btn active" data-tab="overview">Overzicht</button>
                    <button class="tab-btn" data-tab="safety">Veiligheid</button>
                    <button class="tab-btn" data-tab="demontage">Demontage</button>
                    <button class="tab-btn" data-tab="parts">Onderdelen</button>
                    <button class="tab-btn" data-tab="errors">Foutcodes</button>
                    <button class="tab-btn" data-tab="suppliers">Leveranciers</button>
                    <button class="tab-btn" data-tab="links">Links</button>
                </div>

                <div class="panel" id="contentPanel">
                    <div class="empty">Het dashboard staat nog uit. Druk links op de powerknop op de step.</div>
                </div>
            </section>
        </div>
    </div>

    <div class="part-modal" id="partModal">
        <div class="part-modal-card">
            <button class="part-close" id="closePartModal" type="button">×</button>
            <h3 class="part-modal-title" id="partModalTitle">Onderdeel</h3>
            <p class="part-modal-sub" id="partModalSub">Detailweergave van het onderdeel.</p>
            <div class="part-modal-visual">
                <img id="partModalImage" src="" alt="Onderdeel afbeelding">
            </div>
        </div>
    </div>

    <div class="error-image-modal" id="errorPlanModal">
        <button class="error-image-close" id="closeErrorPlanModal" type="button">×</button>
        <img id="errorPlanImage" src="" alt="Stappenplan foutcode">
    </div>

    <script>
        const brandLogos = {
            xiaomi: 'Images steps/Xiaomi.logo.png',
            ninebot: 'Images steps/Nb.logo.png',
            niu: 'Images steps/NIU.logo.png'
        };

        const STEP_IMAGES = {
            g30: 'G30.png',
            d18d38: 'D-serie.png',
            maxg2: 'G2.png',
            f65i: 'F65I.png',
            f40: 'F40.png'
        };

        const ERROR_STEP_IMAGES = {
            '10': 'stappenplan foutcodes/Error 10.png',
            '11': 'stappenplan foutcodes/Error 11,12,13.png',
            '12': 'stappenplan foutcodes/Error 11,12,13.png',
            '13': 'stappenplan foutcodes/Error 11,12,13.png',
            '14': 'stappenplan foutcodes/Error 14.png',
            '15': 'stappenplan foutcodes/Error 15.png',
            '16': 'stappenplan foutcodes/Error 16.png',
            '18': 'stappenplan foutcodes/Error 18.png',
            '19': 'stappenplan foutcodes/Error 19.png',
            '21': 'stappenplan foutcodes/Error 21.png',
            '22': 'stappenplan foutcodes/Error 22.png',
            '23': 'stappenplan foutcodes/Error 23.png',
            '24': 'stappenplan foutcodes/Error 24.png',
            '26': 'stappenplan foutcodes/Error 26.png',
            '27': 'stappenplan foutcodes/Error 27.png',
            '31': 'stappenplan foutcodes/Error 31.png',
            '32': 'stappenplan foutcodes/Error 32.png',
            '35': 'stappenplan foutcodes/Error 35.png',
            '37': 'stappenplan foutcodes/Error 37.png',
            '39': 'stappenplan foutcodes/Error 39.png',
            '40': 'stappenplan foutcodes/Error 40.png',
            '41': 'stappenplan foutcodes/Error 41.png',
            '47': 'stappenplan foutcodes/Error 47.png',
            '48': 'stappenplan foutcodes/Error 48.png',
            '49': 'stappenplan foutcodes/Error 49.png',
            '50': 'stappenplan foutcodes/Error 50.png',
            '51': 'stappenplan foutcodes/Error 51.png',
            '52': 'stappenplan foutcodes/Error 52.png',
            '54': 'stappenplan foutcodes/Error 54.png',
            '55': 'stappenplan foutcodes/Error 55.png'
        };

        function makePartSvg(title, kind) {
            const drawings = {
                battery: `
                                    <rect x="170" y="160" width="300" height="220" rx="30" fill="#121d2f" stroke="#7aa7ff" stroke-width="10"/>
                                    <rect x="470" y="230" width="24" height="80" rx="10" fill="#7aa7ff"/>
                                    <rect x="210" y="205" width="210" height="130" rx="16" fill="#1d3357" stroke="#57d0ff" stroke-width="6"/>
                                    <rect x="225" y="220" width="160" height="100" rx="12" fill="#59dcff" opacity=".25"/>
                                    <text x="320" y="285" text-anchor="middle" fill="#dff2ff" font-size="52" font-family="Arial" font-weight="700">⚡</text>
                                `,
                display: `
                                    <rect x="180" y="120" width="280" height="360" rx="40" fill="#0f1828" stroke="#7aa7ff" stroke-width="10"/>
                                    <rect x="215" y="175" width="210" height="180" rx="22" fill="#163d77" stroke="#59dcff" stroke-width="6"/>
                                    <text x="320" y="265" text-anchor="middle" fill="#dff2ff" font-size="70" font-family="Arial" font-weight="900">25</text>
                                    <circle cx="320" cy="410" r="26" fill="#0a1322" stroke="#7aa7ff" stroke-width="6"/>
                                `,
                controller: `
                                    <rect x="170" y="170" width="300" height="220" rx="24" fill="#141f32" stroke="#7aa7ff" stroke-width="10"/>
                                    <rect x="215" y="215" width="210" height="130" rx="18" fill="#20375b"/>
                                    <line x1="160" y1="225" x2="95" y2="225" stroke="#59dcff" stroke-width="10"/>
                                    <line x1="160" y1="275" x2="95" y2="275" stroke="#59dcff" stroke-width="10"/>
                                    <line x1="160" y1="325" x2="95" y2="325" stroke="#59dcff" stroke-width="10"/>
                                    <line x1="480" y1="225" x2="545" y2="225" stroke="#59dcff" stroke-width="10"/>
                                    <line x1="480" y1="275" x2="545" y2="275" stroke="#59dcff" stroke-width="10"/>
                                    <line x1="480" y1="325" x2="545" y2="325" stroke="#59dcff" stroke-width="10"/>
                                    <text x="320" y="290" text-anchor="middle" fill="#dff2ff" font-size="44" font-family="Arial" font-weight="700">ESC</text>
                                `,
                adapter: `
                                    <rect x="180" y="180" width="210" height="180" rx="28" fill="#10192a" stroke="#7aa7ff" stroke-width="10"/>
                                    <rect x="390" y="235" width="120" height="18" rx="9" fill="#59dcff"/>
                                    <rect x="390" y="285" width="120" height="18" rx="9" fill="#59dcff"/>
                                    <rect x="220" y="145" width="18" height="40" rx="8" fill="#dff2ff"/>
                                    <rect x="270" y="145" width="18" height="40" rx="8" fill="#dff2ff"/>
                                    <text x="285" y="280" text-anchor="middle" fill="#dff2ff" font-size="44" font-family="Arial" font-weight="700">AC</text>
                                `,
                motor: `
                                    <circle cx="320" cy="280" r="130" fill="#10192a" stroke="#7aa7ff" stroke-width="12"/>
                                    <circle cx="320" cy="280" r="78" fill="#1b3152" stroke="#59dcff" stroke-width="8"/>
                                    <circle cx="320" cy="280" r="28" fill="#dff2ff"/>
                                    <path d="M320 110 L340 150 L300 150 Z" fill="#59dcff"/>
                                    <path d="M490 280 L450 300 L450 260 Z" fill="#59dcff"/>
                                    <path d="M320 450 L340 410 L300 410 Z" fill="#59dcff"/>
                                    <path d="M150 280 L190 300 L190 260 Z" fill="#59dcff"/>
                                `
            };

            return 'data:image/svg+xml;utf8,' + encodeURIComponent(`
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 640">
                                    <defs>
                                        <linearGradient id="bg" x1="0" x2="1" y1="0" y2="1">
                                            <stop offset="0%" stop-color="#10203a"/>
                                            <stop offset="100%" stop-color="#06101f"/>
                                        </linearGradient>
                                    </defs>
                                    <rect width="640" height="640" rx="34" fill="url(#bg)"/>
                                    <circle cx="320" cy="320" r="220" fill="rgba(93,143,255,.10)"/>
                                    ${drawings[kind]}
                                    <text x="320" y="560" text-anchor="middle" fill="#dce8ff" font-size="34" font-family="Arial" font-weight="700">${title}</text>
                                </svg>
                            `);
        }

        const PART_IMAGES = {
            fallback: {
                batterij: makePartSvg('Batterij', 'battery'),
                display: makePartSvg('Display', 'display'),
                controller: makePartSvg('Controller', 'controller'),
                adapter: makePartSvg('Adapter', 'adapter'),
                naafmotor: makePartSvg('Naafmotor', 'motor')
            },
            G30: {
                batterij: 'Foto onderdelen/Batterij.G30.png',
                display: 'Foto onderdelen/Display.G30.png',
                controller: 'Foto onderdelen/Controller.G30.png',
                adapter: 'Foto onderdelen/Adapter.G30.png',
                naafmotor: 'Foto onderdelen/Naafmotor.png'
            },
            'D18-D38': {
                batterij: 'D18/Batterij.D18.png',
                display: 'D18/Display.D18.png',
                controller: 'D18/Controller.D18.png',
                adapter: 'D18/Adapter.D18.png',
                naafmotor: 'D18/Naafmotor.D18.png'
            },
            'MAX G2': {
                batterij: 'MAXG2/Batterij.G2.png',
                display: 'MAXG2/Display.G2.png',
                controller: 'MAXG2/Controller.G2.png',
                adapter: 'MAXG2/Adapter.G2.png',
                naafmotor: 'MAXG2/Naafmotor.G2.png'
            },
            F65I: {
                batterij: 'F65I/Batterij.F65I.png',
                display: 'F65I/Display.F65I.png',
                controller: 'F65I/Controller.F65I.png',
                adapter: 'F65I/Adapter.F65I.png',
                naafmotor: 'F65I/Naafmotor.F65I.png'
            },
            F40: {
                batterij: 'F40/Batterij.F40.png',
                display: 'F40/Display.F40.png',
                controller: 'F40/Controller.F40.png',
                adapter: 'F40/Adapter.F40.png',
                naafmotor: 'F40/Naafmotor.F40.png'
            }
        };

        const COMMON_ERROR_CODES = [
            ['10', 'Communicatieprobleem tussen dashboard en controller', 'Controleer kabelboom tussen display en controller. Meet continuïteit en controleer op corrosie, losse connectoren of slecht contact. Reset de step eerst en controleer daarna alle stekkers opnieuw.', 'Communicatie via seriële lijn tussen display PCB en controller (ESC) is onderbroken of instabiel.', 'https://docs.google.com/document/d/1uHRrztUz3VkDjmNpzk62ufIQTHHd2_kG8kbrc-FunO0/edit?usp=drive_link'],
            ['11', 'Motor fase A / Hall-signaal fout', 'Maak de step spanningsloos, open het dek en controleer de motorconnector. Kijk naar pinnen, vocht, corrosie, verbrande contacten en slecht contact in de Hall-sensor verbinding.', 'Hall-sensor fout of motorconnectorprobleem.', 'https://docs.google.com/document/d/1RBKEbSZoeiRSgakj3dRWpuUyh2CCZMXGBh7vIy2vCAk/edit?usp=drive_link'],
            ['12', 'Motor fase B fout', 'Controleer de drie dikke fasekabels tussen motor en controller. Kijk op verbrande stekkers, gesmolten isolatie, oxidatie en losse verbindingen.', 'Fasekabel probleem tussen motor en controller.', 'https://docs.google.com/document/d/1i8-HJqJ8x99VuKqgdQp2AjOiEeHo6o08MfNSkfuKXVA/edit?usp=drive_link'],
            ['13', 'Abnormale stroommeting op motorfase C', 'Controleer eerst de fasekabel van de motor op schade, knikken of breuken. Als de kabel in orde is, vervang de controller. Blijft de fout, vervang dan de motor.', 'De controller meet de stroom op motorfase C niet correct.', 'https://docs.google.com/document/d/1MN2glqHa_lD4aOC7ixlZU6q6v6eBelt422FA_OrvQeU/edit?usp=drive_link'],
            ['14', 'Gashendel Hall-sensor fout', 'Controleer kabel tussen throttle en display, meet Hall-signaal en controleer connectoren.', 'Throttle-signaal buiten verwacht bereik.', 'https://docs.google.com/document/d/1iHiMNFVoGyoQt5eECRWRai4UDjTtZJE6r7BZQtypSD8/edit?usp=drive_link'],
            ['15', 'Remhendel / remsensor fout', 'Controleer remsensor, kabelverbinding en magneet. Reset de step en test opnieuw.', 'Remsignaal blijft actief of wordt niet correct gelezen.', 'https://docs.google.com/document/d/1iHiMNFVoGyoQt5eECRWRai4UDjTtZJE6r7BZQtypSD8/edit?usp=drive_link'],
            ['16', 'Interne batterij MOSFET fout', 'Controleer accu en interne MOSFET’s. Als de BMS de fout blijft geven, is batterijvervanging vaak nodig.', 'Charge/discharge circuit in het batterijpakket werkt niet correct.', 'https://docs.google.com/document/d/1Vr5r4vWk0qeIMDDZeGNqMZQ0u6qXWkdOPuTI-QyuM6g/edit?usp=drive_link'],
            ['17', 'Externe batterij MOSFET fout', 'Controleer de externe batterijmodule, ontlaad- of laadpad en de verbinding met de controller.', 'Fout in externe batterij MOSFET schakeling.', 'https://docs.google.com/document/d/1dWZhIM7i81uwuwVSuDUfb1uOBJ6p2d-_Hb0PIEEaegA/edit?usp=drive_link'],
            ['18', 'Motor Hall-sensor fout', 'Controleer Hall-sensor signalen, motorkabel en sensorconnectoren. Meet of de signalen stabiel schakelen tussen laag en hoog.', 'Motor Hall-sensor signaal ontbreekt of is inconsistent.', 'https://docs.google.com/document/d/1bv80TBIUqSLm-lrtMeHIRvgAY8zS3NCOAw_ZJ2ej9iU/edit?usp=drive_link'],
            ['19', 'Interne batterijspanning te laag of instabiel', 'Meet batterijspanning richting controller, controleer celspanningen via BMS en kijk of de spanning onder belasting inzakt.', 'Spanningsbewaking van de interne batterij detecteert afwijkingen.', 'https://docs.google.com/document/d/1BMO3A1CeFdHnAz5cznzx1BxZFtqgblMRyazrMe6kNBg/edit?usp=drive_link'],
            ['20', 'Externe batterijspanning onregelmatig', 'Controleer of de batterijspanning te laag, te hoog of instabiel is. Meet onder belasting en controleer BMS en batterijstatus.', 'Controller detecteert foutieve of wegvallende batterijspanning.', 'https://docs.google.com/document/d/1JzUJziqw8MWtgnXNqZyx0rsGszzOZ1Ogkl2QUKFvgqo/edit?usp=drive_link'],
            ['21', 'Communicatie tussen BMS en controller mislukt', 'Controleer datalijn, dunne signaalkabels, BMS-status en verbinding tussen batterij en controller.', 'Geen of foutieve data-uitwisseling tussen batterij en controller.', 'https://docs.google.com/document/d/18TaoErd7y_-dC8QkMPRuJHbXSPBPGtxsMSQtgvDClcA/edit?usp=drive_link'],
            ['22', 'Batterij authenticatie / code fout', 'Controleer of de batterij origineel is, of het serienummer overeenkomt en of de BMS juiste ID-data doorstuurt.', 'Controller accepteert batterij-ID of firmwarecode niet.', 'https://docs.google.com/document/d/1UaYkfuCdXhRdEWShU8HMrnjdlPNNYBWReDEFv-wZXzs/edit?usp=drive_link'],
            ['23', 'Serienummer batterij ongeldig', 'Controleer batterij-ID, BMS-geheugen en authenticatie-informatie.', 'Ongeldig of beschadigd serienummer in de batterij.', 'https://docs.google.com/document/d/1TOOzv_DiD_VKJz7PoFTNGI3UKzxKWwz0oJgKAvw_g_U/edit?usp=drive_link'],
            ['24', 'Systeemspanningsmeting fout', 'Controleer spanningslijn tussen batterij en controller en meet of de controller de juiste spanning ontvangt.', 'Afwijking in spanningsmeting of voedingslijn.', 'https://docs.google.com/document/d/1hmy9REC7RsVJQy1W-meMxcHWJ3s1MsdDtcQW3EhFP8Q/edit?usp=drive_link'],
            ['26', 'Flash / geheugenfout in controller', 'Controller resetten of vervangen indien firmware- of geheugenprobleem bevestigd is.', 'Interne controllergeheugen fout.', 'https://docs.google.com/document/d/1CSwJX_oK6Hfwdp-BdWUndg9FLk1XHal6PnRjLMGeWHQ/edit?usp=drive_link'],
            ['27', 'Firmware / besturingscode fout', 'Controleer software, firmware en of een update of herprogrammering nodig is.', 'Besturingssoftware of firmware corrupt of incompatibel.', 'https://docs.google.com/document/d/1sXBG0tzaN-JwJcIJMqmwqOxpWAQI5g_x0dd_yrG9PRY/edit?usp=drive_link'],
            ['28', 'MOSFET short up-bridge', 'Controleer de controller op defecte vermogens-MOSFET’s en vervang indien nodig.', 'Kortsluiting in bovenste MOSFET-brug.', 'https://docs.google.com/document/d/1eLP3lG79Q3gODyarm1wnQMA2XzkJiso2OOa4obnUf1c/edit?usp=drive_link'],
            ['29', 'MOSFET short down-bridge', 'Controleer vermogensgedeelte van de controller en vervang defecte componenten.', 'Kortsluiting in onderste MOSFET-brug.', 'https://docs.google.com/document/d/178s_CO3Gk4PmCDYAbnQul16riG3dnJ4S4v5aCr5sHlY/edit?usp=drive_link'],
            ['31', 'Programma- of controllerfout', 'Controller herprogrammeren of vervangen na diagnose.', 'Interne controller- of softwarefout.', 'https://docs.google.com/document/d/19WiZ4ynX4kJABA08i8A7azuyOn6SgnwEHGUtYTLhVoM/edit?usp=drive_link'],
            ['35', 'Standaard serienummer in controller of batterij', 'Controleer serienummer, firmware en board-authenticatie. Indien nodig moederbord of batterij vervangen.', 'Controller of batterij bevat standaard of fabrieks-ID.', 'https://docs.google.com/document/d/1lda5PMS8yJFiBo5uuy6suzY8hCFH__AuYx4VZlmof-E/edit?usp=drive_link'],
            ['39', 'Batterijtemperatuur buiten limiet', 'Controleer temperatuursensor, batterijtemperatuur en koeling.', 'Temperatuur van batterij te hoog of te laag.', 'https://docs.google.com/document/d/1N9KB1OGmYcYwbY1f7KpqefbdCJj_xLghOWh2k-ovQ54/edit?usp=drive_link'],
            ['40', 'Controller temperatuursensor fout', 'Controleer temperatuursensor op ESC/controller.', 'Temperatuurmeting controller foutief.', 'https://docs.google.com/document/d/1AoECe3TwhxUgdl-vdXSHb81FzhSvYUbuCWFxfeg3xa8/edit?usp=drive_link'],
            ['41', 'Externe batterij temperatuur fout', 'Controleer externe batterij sensor en verbindingen.', 'Temperatuurfout in externe batterij.', 'https://docs.google.com/document/d/1dWm9eYyBejekFhg6TgjxgdqKoClQov2q4Fjvp37lYOM/edit?usp=drive_link'],
            ['42', 'Externe batterij communicatie fout', 'Controleer bedrading, datalijn en verbinding met externe accu.', 'Geen stabiele communicatie met externe batterij.', 'https://docs.google.com/document/d/15frA8-A6Satnb--WJeNmqOl4WP9Afz6djbHo9ebf5bA/edit?usp=drive_link'],
            ['43', 'Externe batterij code mismatch', 'Controleer batterij-ID, batterijtype, compatibiliteit en firmware.', 'Externe batterij komt niet overeen met controllerverwachting.', 'https://docs.google.com/document/d/1PR8_loF_fe5fdcLac551fCe-w16ZAg55o3ZGvyVIh5M/edit?usp=drive_link'],
            ['44', 'Externe batterij serienummer standaard', 'Controller blokkeert batterij; vaak is vervanging of herprogrammering nodig.', 'Externe batterij heeft standaard of test-serienummer.', 'https://docs.google.com/document/d/1To-kZuMzuDBCZSlZa1hkiDWSBqGIAHBp5xycdXeW8bk/edit?usp=drive_link'],
            ['45', 'Interne BMS diepe ontlading', 'Controleer individuele celspanningen via BMS. Vaak is één zwakke cel de oorzaak.', 'Minstens één cel is onder veilige minimumspanning gezakt.', 'https://docs.google.com/document/d/1VujA2I4Sp0walqytb046slHQadKvPKXDQsmo6e6B3cQ/edit?usp=drive_link'],
            ['46', 'Externe BMS diepe ontlading', 'Controleer externe batterijcellen, laad de batterij op en controleer of één cel de pack blokkeert.', 'Diepe ontlading van externe batterij.', 'https://docs.google.com/document/d/14GoAaT3dWqMee6sr83-QNaWMJUMN8YzUuevxL04XJ58/edit?usp=drive_link'],
            ['47', 'Interne BMS communicatie fout', 'Controleer communicatiekabel tussen BMS en controller, inclusief TX/RX of datalijn.', 'Controller ontvangt geen bruikbare data van interne BMS.', 'https://docs.google.com/document/d/1oBAmXCg9kUaPlPfdxtXyEcckqoj4ccndiM1nt7ClDdo/edit?usp=drive_link'],
            ['48', 'Externe BMS communicatie fout', 'Controleer verbinding met externe BMS en alle dunne signaalkabels.', 'Geen communicatie met externe batterij-BMS.', 'https://docs.google.com/document/d/1ZYKl5u4QFgf7e_eYQefDaoJ-ki3HURAa4btJENblvVE/edit?usp=drive_link'],
            ['49', 'Interne BMS firmware mismatch', 'Controleer firmwareversies van BMS en controller en kijk of update of compatibiliteitscheck nodig is.', 'Interne BMS-firmware komt niet overeen met controllerverwachting.', 'https://docs.google.com/document/d/1HoRrkIg8KRsYXzbyLJrwJXQSQcNv_Mox6Gvm92L07W0/edit?usp=drive_link'],
            ['50', 'Externe BMS firmware mismatch', 'Controleer firmwarecompatibiliteit van externe batterij en controller.', 'Firmware van externe BMS is niet compatibel.', 'https://docs.google.com/document/d/1RCQrMQt1kW3qrLsKt5Xf-5jMR9JvqB2ov3fmW8mOMOc/edit?usp=drive_link']
        ];

        function cloneErrors() {
            return COMMON_ERROR_CODES.map(item => [...item]);
        }

        function buildCommonParts(overrides = {}) {
            return [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: overrides.batterijStock || 'Goed',
                    description: overrides.batterijDescription || 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    price: overrides.batterijPrice || '€240 - €295',
                    delivery: overrides.batterijDelivery || '3-4 weken',
                    testLink: overrides.batterijTestLink || 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: overrides.displayStock || 'Goed',
                    description: overrides.displayDescription || 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    price: overrides.displayPrice || '€48,95 - €79,00',
                    delivery: overrides.displayDelivery || '2-5 werkdagen',
                    testLink: overrides.displayTestLink || 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: overrides.controllerStock || 'Gemiddeld',
                    description: overrides.controllerDescription || 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    price: overrides.controllerPrice || '€100,00 - €176,95',
                    delivery: overrides.controllerDelivery || '2-5 werkdagen',
                    testLink: overrides.controllerTestLink || 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: overrides.adapterStock || 'Goed',
                    description: overrides.adapterDescription || 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    price: overrides.adapterPrice || '€59,00 - €119,00',
                    delivery: overrides.adapterDelivery || '2-5 werkdagen',
                    testLink: overrides.adapterTestLink || 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: overrides.motorStock || 'Gemiddeld',
                    description: overrides.motorDescription || 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    price: overrides.motorPrice || '€199,00',
                    delivery: overrides.motorDelivery || '2-8 weken',
                    testLink: overrides.motorTestLink || 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ];
        }

        function buildRepairs(modelName) {
            return [
                {
                    title: 'Resetprocedure',
                    text: `Bij ${modelName} is een eerste stap om de step volledig uit te schakelen, even te wachten en opnieuw op te starten. Tijdelijke communicatieproblemen kunnen zo verdwijnen.`
                },
                {
                    title: 'Connectoren en bekabeling',
                    text: 'Veel foutcodes wijzen op problemen in kabels, stekkers, Hall-sensorlijnen, motorfasekabels of communicatie tussen batterij, display en controller. Controle op corrosie, losse connectoren en verbrande contacten is essentieel.'
                },
                {
                    title: 'Batterij en BMS',
                    text: 'Bij foutcodes rond spanning, diepe ontlading, authenticatie of firmware moet je de batterij, de celspanningen en de BMS-communicatie controleren. Bij ernstige afwijkingen is batterijvervanging soms de veiligste oplossing.'
                },
                {
                    title: 'Controller en firmware',
                    text: 'Bij interne controllerfouten, firmware mismatch of MOSFET-problemen moet je controleren of de controller nog correct werkt, of software/firmware overeenkomt en of vervanging nodig is.'
                },
                {
                    title: 'Motor en sensoren',
                    text: 'Bij foutcodes voor Hall-sensoren, motorfasen of throttle-signalen controleer je de motorconnector, sensoren, gashendel en remsensoren. Meten van signalen en visuele inspectie zijn hier belangrijk.'
                }
            ];
        }

        const g30 = {
            overview: {
                model: 'Ninebot Max G30 (E II)',
                brand: 'Segway-Ninebot',
                series: 'G-serie',
                category: 'Elektrische step / e-scooter',
                battery: 'Lithium-ion batterij 551 Wh (ongeveer 15.300 mAh), spanning ±36V',
                range: 'Tot ongeveer 65 km per lading',
                charge: 'Ongeveer 6 uren',
                motor: 'Nominaal 350W, piek tot ongeveer 700W',
                speed: 'Tot 25 km/h (EU)',
                climb: 'Tot ongeveer 20% helling',
                drive: 'Achterwielaandrijving',
                wheels: '10-inch tubeless luchtbanden met jelly layer',
                brakes: 'Elektrische / regeneratieve achterrem + trommelrem voor',
                sizeOpen: '±1167 × 472 × 1203 mm',
                sizeClosed: '±1167 × 472 × 534 mm',
                weight: '±19,9 kg',
                maxLoad: '±100 kg',
                display: 'Kleuren-LCD dashboard',
                rideModes: 'Eco, Drive, Sport + Walk mode',
                app: 'Bluetooth / smartphone app mogelijk',
                lights: 'Ingebouwd voor- en achterlicht'
            },
            safetyLink: 'https://docs.google.com/document/d/1JQy1xKX26iK3P21l9ayXARRpe77ZVU0nAEnQRTbOoqk/edit?usp=sharing',
            demontageLink: 'https://docs.google.com/document/d/1CeWhees6VUAtyHafVgk9j42vYUrImjtNy_33Ucr1fZw/edit?usp=drive_link',
            partsGuideLink: 'https://docs.google.com/document/d/17NKRjwLBcumy71j4KWEwQEPmkmlCSH3W8hgzl_42gek/edit?usp=drive_link',
            theory: [
                'Hoofdcomponenten: lithium-ion batterijpakket 36V met BMS, motorcontroller (ESC), BLDC-naafmotor met Hall-sensoren en dashboardmodule.',
                'Energiestroom: batterij 36–42V DC → BMS → controller → BLDC-motor → wiel.',
                'Informatiestroom: Hall-sensoren → controller, throttle → controller, rem → controller, BMS → controller.',
                'Batterijpakket in 10S configuratie, maximaal ongeveer 42V.',
                'Dikke kabels zijn voor vermogen; dunne kabels voor communicatie en sensoren.'
            ],
            links: [
                { label: 'Segway productpagina', href: 'https://be-nl.segway.com/products/ninebot-kickscooter-max-g30-1' },
                { label: 'Review video', href: 'https://www.youtube.com/watch?v=QV1AMtWhx4c' },
                { label: 'Reset video', href: 'https://www.youtube.com/watch?v=27OQ4qUkWA4' },
                { label: 'Algemene error links', href: 'https://nr1elektrischestep.nl/blogs/elektrische-step/segway-ninebot-error-codes?utm_source=chatgpt.com' }
            ],
            parts: [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: 'Goed',
                    description: 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    testLink: 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: 'Goed',
                    description: 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    testLink: 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: 'Gemiddeld',
                    description: 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    testLink: 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: 'Goed',
                    description: 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    testLink: 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: 'Gemiddeld',
                    description: 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    testLink: 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ],
            errorCodes: cloneErrors(),
            repairs: buildRepairs('de Ninebot MAX G30')
        };

        const d18d38 = {
            overview: {
                model: 'Ninebot D18 / D38',
                brand: 'Segway-Ninebot',
                series: 'D-serie',
                category: 'Elektrische step / e-scooter',
                battery: 'Lithium-ion batterij 183 Wh tot 367 Wh, spanning ±36V',
                range: 'Tot ongeveer 18 km (D18) en 38 km (D38)',
                charge: 'Ongeveer 3,5 tot 6,5 uren',
                motor: 'Nominaal 250W tot 350W',
                speed: 'Tot 25 km/h (EU)',
                climb: 'Tot ongeveer 15% helling',
                drive: 'Voor- of achterwielaandrijving afhankelijk van uitvoering',
                wheels: '10-inch luchtbanden',
                brakes: 'Elektronische rem + trommelrem',
                sizeOpen: '±1143 × 480 × 1160 mm',
                sizeClosed: '±1143 × 480 × 495 mm',
                weight: '±16,3 kg tot ±17,8 kg',
                maxLoad: '±120 kg',
                display: 'LED-dashboard',
                rideModes: 'Eco, Drive, Sport + Walk mode',
                app: 'Bluetooth / Segway-Ninebot app',
                lights: 'Voor- en achterlicht ingebouwd'
            },
            safetyLink: 'https://docs.google.com/document/d/1DrWeUvBrSIbsrAgyETFTzjPytbbQzpRG6SpMj1ceCAU/edit?usp=drive_link',
            demontageLink: 'https://docs.google.com/document/d/1acpCfoFqz8EtfIQZGPj7tYoOfQVtF-bb/edit?usp=drive_link&ouid=115571632246130091461&rtpof=true&sd=true',
            partsGuideLink: 'https://docs.google.com/document/d/1CW867OmFcPpYRYlu734Vg-r_CI4BzuAgqe2cNGjJ3PE/edit?usp=drive_link',
            theory: [
                'D-serie is ontworpen als commuter-step met focus op eenvoud, bereik en dagelijks gebruik.',
                'Batterij, controller, display en naafmotor volgen hetzelfde basisprincipe als de G30.',
                'De D18 is compacter en lichter, de D38 heeft groter bereik en meestal hogere accucapaciteit.',
                'Signaallijnen voor rem, throttle en display lopen via de stuurkabelboom.',
                '10-inch luchtbanden zorgen voor meer comfort dan compacte solid tires.'
            ],
            links: [
                { label: 'Segway productpagina', href: 'https://be-nl.segway.com/products/ninebot-kickscooter-d-series' },
                { label: 'Review video', href: 'https://www.youtube.com/watch?v=nLY7lOdyouw' },
                { label: 'Reset video', href: 'https://www.youtube.com/watch?v=27OQ4qUkWA4' }
            ],
            parts: [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: 'Goed',
                    description: 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    testLink: 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: 'Goed',
                    description: 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    testLink: 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: 'Gemiddeld',
                    description: 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    testLink: 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: 'Goed',
                    description: 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    testLink: 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: 'Gemiddeld',
                    description: 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    testLink: 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ],
            errorCodes: cloneErrors(),
            repairs: buildRepairs('de Ninebot D18 / D38')
        };

        const maxG2 = {
            overview: {
                model: 'Ninebot MAX G2',
                brand: 'Segway-Ninebot',
                series: 'G-serie',
                category: 'Elektrische step / e-scooter',
                battery: 'Lithium-ion batterij 551 Wh, spanning ±36V',
                range: 'Tot ongeveer 70 km per lading',
                charge: 'Ongeveer 6 uren',
                motor: 'Nominaal 450W, piek hoger bij acceleratie',
                speed: 'Tot 25 km/h (EU)',
                climb: 'Tot ongeveer 22% helling',
                drive: 'Achterwielaandrijving',
                wheels: '10-inch tubeless luchtbanden',
                brakes: 'Dubbele remcombinatie met elektronische rem en trommelrem',
                sizeOpen: '±1210 × 570 × 1264 mm',
                sizeClosed: '±1210 × 570 × 605 mm',
                weight: '±24,3 kg',
                maxLoad: '±120 kg',
                display: 'Geïntegreerd dashboard',
                rideModes: 'Eco, Drive, Sport, Walk',
                app: 'Bluetooth / Segway-Ninebot app',
                lights: 'Voor-, achter- en richtingverlichting'
            },
            safetyLink: 'https://docs.google.com/document/d/1cfoD2TcJymTQfCYeK0JKmH5TaY6YIlJ6/edit?usp=drive_link&ouid=115571632246130091461&rtpof=true&sd=true',
            demontageLink: 'https://docs.google.com/document/d/1SNnJIUI9uELqnJ8rCUm-SkFz2wC8ymARtfREGOiVjmk/edit?usp=drive_link',
            partsGuideLink: 'https://docs.google.com/document/d/1qCEQaOP8QRDFecZn9CuYXnPjJxtxfzGc-_1X_3loAik/edit?usp=drive_link',
            theory: [
                'MAX G2 bouwt verder op de MAX-lijn maar met krachtigere motor en meer comfort.',
                'Vering en sterkere commuter-opbouw maken dit model zwaarder dan de G30.',
                'Batterij- en controllersysteem werken nog steeds rond een 36V-architectuur met BMS.',
                'App-connectiviteit en slimme functies zijn uitgebreider dan bij oudere G-modellen.',
                'De step is bedoeld voor langere ritten met meer stabiliteit en rijcomfort.'
            ],
            links: [
                { label: 'Segway productpagina', href: 'https://be-nl.segway.com/products/ninebot-kickscooter-max-g2e-powered-by-segway' },
                { label: 'Review video', href: 'https://www.youtube.com/watch?v=xwY1oHACfGw' },
                { label: 'Reset video', href: 'https://www.youtube.com/watch?v=27OQ4qUkWA4' }
            ],
            parts: [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: 'Goed',
                    description: 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    testLink: 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: 'Goed',
                    description: 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    testLink: 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: 'Gemiddeld',
                    description: 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    testLink: 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: 'Goed',
                    description: 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    testLink: 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: 'Gemiddeld',
                    description: 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    testLink: 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ],
            errorCodes: cloneErrors(),
            repairs: buildRepairs('de Ninebot MAX G2')
        };
        const f65i = {
            overview: {
                model: 'Ninebot F65I',
                brand: 'Segway-Ninebot',
                series: 'F-serie',
                category: 'Elektrische step / e-scooter',
                battery: 'Lithium-ion batterij met groot bereik, spanning ±36V',
                range: 'Tot ongeveer 65 km per lading',
                charge: 'Ongeveer 5 tot 6,5 uren',
                motor: 'Nominaal 400W',
                speed: 'Tot 25 km/h (EU)',
                climb: 'Tot ongeveer 20% helling',
                drive: 'Voorwielaandrijving',
                wheels: '10-inch luchtbanden',
                brakes: 'Schijfrem + elektronische rem',
                sizeOpen: '±1160 × 510 × 1230 mm',
                sizeClosed: '±1160 × 510 × 540 mm',
                weight: '±21,3 kg',
                maxLoad: '±120 kg',
                display: 'LED-dashboard',
                rideModes: 'Eco, Drive, Sport, Walk',
                app: 'Bluetooth / Segway-Ninebot app',
                lights: 'Ingebouwde verlichting voor en achter'
            },
            safetyLink: '#',
            demontageLink: 'https://docs.google.com/document/d/1qj4qfd50uMe6Qmt5LhdTkxRx9_us8hDCperoZykNpMs/edit?usp=drive_link',
            partsGuideLink: 'https://docs.google.com/document/d/1nyVY-C9M_jw22NZyJbVAgl2nylsungGqcDjNozgFTrE/edit?usp=drive_link',
            theory: [
                'De F65I behoort tot de F-serie en combineert groter bereik met een slanker frame dan de MAX-lijn.',
                '10-inch wielen en schijfrem maken het model geschikt voor dagelijks woon-werkverkeer.',
                'Batterij, controller, dashboard en motor blijven volgens het standaard Ninebot-principe samenwerken.',
                'De F-serie heeft vaak een modernere, lichtere frame-uitstraling dan de G-serie.',
                'Diagnose van foutcodes loopt grotendeels via dezelfde controller- en BMS-logica.'
            ],
            links: [
                { label: 'Segway productpagina', href: 'https://be-nl.segway.com/products/ninebot-kickscooter-f65i' },
                { label: 'Review video', href: 'https://www.youtube.com/watch?v=MsSqrVUdyLI' },
                { label: 'Reset video', href: 'https://www.youtube.com/watch?v=27OQ4qUkWA4' }
            ],
            parts: [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: 'Goed',
                    description: 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    testLink: 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: 'Goed',
                    description: 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    testLink: 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: 'Gemiddeld',
                    description: 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    testLink: 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: 'Goed',
                    description: 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    testLink: 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: 'Gemiddeld',
                    description: 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    testLink: 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ],
            errorCodes: cloneErrors(),
            repairs: buildRepairs('de Ninebot F65I')
        };

        const f40 = {
            overview: {
                model: 'Ninebot F40',
                brand: 'Segway-Ninebot',
                series: 'F-serie',
                category: 'Elektrische step / e-scooter',
                battery: 'Lithium-ion batterij ±367 Wh, spanning ±36V',
                range: 'Tot ongeveer 40 km per lading',
                charge: 'Ongeveer 6,5 uren',
                motor: 'Nominaal 350W',
                speed: 'Tot 25 km/h (EU)',
                climb: 'Tot ongeveer 20% helling',
                drive: 'Voorwielaandrijving',
                wheels: '10-inch luchtbanden',
                brakes: 'Schijfrem + elektronische rem',
                sizeOpen: '±1148 × 480 × 1160 mm',
                sizeClosed: '±1148 × 480 × 495 mm',
                weight: '±17,1 kg',
                maxLoad: '±120 kg',
                display: 'LED-dashboard',
                rideModes: 'Eco, Drive, Sport, Walk',
                app: 'Bluetooth / Segway-Ninebot app',
                lights: 'Voor- en achterlicht ingebouwd'
            },
            safetyLink: 'https://docs.google.com/document/d/11eYezXtRavZOFcFGbsjcBAXEV2fhfUFfk4H6AIR4Soc/edit?usp=drive_link',
            demontageLink: 'https://docs.google.com/document/d/1tpZvGdn5RmWQhXFG1eAn5kGoW0pjFM5G2XN81vAwFIA/edit?usp=drive_link',
            partsGuideLink: 'https://docs.google.com/document/d/1EKmR7C3JohiCiSl7ULQdeFVOV7qaC8-oSXRi-ELDYRs/edit?usp=drive_link',
            theory: [
                'F40 is een middenmodel binnen de F-serie met focus op stedelijk gebruik en degelijk bereik.',
                'De combinatie van 10-inch banden en schijfrem maakt hem geschikt als dagelijkse commuter-step.',
                'De elektrische architectuur blijft gebaseerd op batterij, BMS, controller, dashboard en naafmotor.',
                'Het frame is slanker dan de MAX-reeks maar nog steeds stabiel voor regulier gebruik.',
                'Foutdiagnose volgt grotendeels dezelfde Ninebot-logica als andere moderne modellen.'
            ],
            links: [
                { label: 'Segway productpagina', href: 'https://be-nl.segway.com/products/ninebot-kickscooter-f40e-powered-by-segway' },
                { label: 'Review video', href: 'https://www.youtube.com/watch?v=k3-URwEdsw0' },
                { label: 'Reset video', href: 'https://www.youtube.com/watch?v=27OQ4qUkWA4' }
            ],
            parts: [
                {
                    key: 'batterij',
                    name: 'Batterij',
                    stock: 'Goed',
                    description: 'Levert en bewaart energie voor motor en elektronica via het BMS.',
                    testLink: 'https://docs.google.com/document/d/1s8DDhQFbiPYOf5LtDlv05Twmu56SHWjVs8jl87dHdos/edit?usp=drive_link'
                },
                {
                    key: 'display',
                    name: 'Display',
                    stock: 'Goed',
                    description: 'Geeft laadtoestand, percentage en snelheid van de step weer.',
                    testLink: 'https://docs.google.com/document/d/1vV4704_6cMv9RjW1q0h32AVqYZq-vSV1js3r2WWqlds/edit?usp=drive_link'
                },
                {
                    key: 'controller',
                    name: 'Controller',
                    stock: 'Gemiddeld',
                    description: 'Stuurt de motor aan en verwerkt signalen van gas, remmen en sensoren.',
                    testLink: 'https://docs.google.com/document/d/1klBTd7jKH1y_2GCLzLoLxhrNsQ_ymXKxZ2MnrAvp4_4/edit?usp=drive_link'
                },
                {
                    key: 'adapter',
                    name: 'Adapter',
                    stock: 'Goed',
                    description: 'Zet stroom van het stopcontact om en regelt het veilig opladen van de batterij.',
                    testLink: 'https://docs.google.com/document/d/1lON4g2AarI9DVwRXEGhJJ5clTSXxb-xeodvO8efp0_c/edit?usp=drive_link'
                },
                {
                    key: 'naafmotor',
                    name: 'Naafmotor',
                    stock: 'Gemiddeld',
                    description: 'Elektrische motor in het wiel die het wiel direct aandrijft.',
                    testLink: 'https://docs.google.com/document/d/1PM-CTZQ1TBF_8juGPmDQTP8wavvDwPp3cljAGTnWy2k/edit?usp=drive_link'
                }
            ],
            errorCodes: cloneErrors(),
            repairs: buildRepairs('de Ninebot F40')
        };

        const modelsByBrand = {
            ninebot: [
                { name: 'Ninebot MAX G30', code: 'G30', thumb: 'Ninebot MAX G30', image: STEP_IMAGES.g30, status: 'Model actief', data: g30 },
                { name: 'Ninebot D18 / D38', code: 'D18-D38', thumb: 'Ninebot D18 / D38', image: STEP_IMAGES.d18d38, status: 'Model actief', data: d18d38 },
                { name: 'Ninebot MAX G2', code: 'MAX G2', thumb: 'Ninebot MAX G2', image: STEP_IMAGES.maxg2, status: 'Model actief', data: maxG2 },
                { name: 'Ninebot F65I', code: 'F65I', thumb: 'Ninebot F65I', image: STEP_IMAGES.f65i, status: 'Model actief', data: f65i },
                { name: 'Ninebot F40', code: 'F40', thumb: 'Ninebot F40', image: STEP_IMAGES.f40, status: 'Model actief', data: f40 }
            ],
            xiaomi: [],
            niu: []
        };

        const brands = [
            { key: 'xiaomi', name: 'Xiaomi', sub: 'Merkenoverzicht • nog geen modelinfo toegevoegd' },
            { key: 'ninebot', name: 'Ninebot', sub: 'Merk met 5 huidige modellen in jouw project' },
            { key: 'niu', name: 'NIU', sub: 'Merkenoverzicht • nog geen modelinfo toegevoegd' }
        ];

        let poweredOn = false;
        let activeBrand = null;
        let activeIndex = null;
        let activeTab = 'overview';
        let currentView = 'dashboard';
        let dashboardStage = 'brands';

        let aiPhotoName = '';
        let aiPhotoDataUrl = '';
        let aiDescription = '';
        let aiSuspension = 'unknown';
        let aiRedAccents = 'unknown';
        let aiFrameSize = 'unknown';
        let aiWheelSize = 'unknown';
        let aiStemShape = 'unknown';
        let aiDeckType = 'unknown';

        const sleep = (ms) => new Promise(resolve => setTimeout(resolve, ms));

        let systemState, cockpit, powerButton, bootBox, bootFill, screenLabel, screenNumber, screenMode, screenSub;
        let titleModel, titleDesc, modelStatus, contentPanel, tabButtons;
        let dashboardView, contentView, backToDashboard, stagePanel;
        let partModal, partModalTitle, partModalSub, partModalImage, closePartModal;
        let errorPlanModal, errorPlanImage, closeErrorPlanModal;

        function initRefs() {
            systemState = document.getElementById('systemState');
            cockpit = document.getElementById('cockpit');
            powerButton = document.getElementById('powerButton');
            bootBox = document.getElementById('bootBox');
            bootFill = document.getElementById('bootFill');
            screenLabel = document.getElementById('screenLabel');
            screenNumber = document.getElementById('screenNumber');
            screenMode = document.getElementById('screenMode');
            screenSub = document.getElementById('screenSub');
            titleModel = document.getElementById('titleModel');
            titleDesc = document.getElementById('titleDesc');
            modelStatus = document.getElementById('modelStatus');
            contentPanel = document.getElementById('contentPanel');
            dashboardView = document.getElementById('dashboardView');
            contentView = document.getElementById('contentView');
            backToDashboard = document.getElementById('backToDashboard');
            stagePanel = document.getElementById('stagePanel');
            tabButtons = [...document.querySelectorAll('.tab-btn')];

            partModal = document.getElementById('partModal');
            partModalTitle = document.getElementById('partModalTitle');
            partModalSub = document.getElementById('partModalSub');
            partModalImage = document.getElementById('partModalImage');
            closePartModal = document.getElementById('closePartModal');

            errorPlanModal = document.getElementById('errorPlanModal');
            errorPlanImage = document.getElementById('errorPlanImage');
            closeErrorPlanModal = document.getElementById('closeErrorPlanModal');
        }

        function setTabsState() {
            tabButtons.forEach(btn => btn.classList.toggle('active', btn.dataset.tab === activeTab));
        }

        function getActiveStep() {
            if (!activeBrand) return null;
            const arr = modelsByBrand[activeBrand] || [];
            if (activeIndex === null || activeIndex === undefined) return null;
            return arr[activeIndex] || null;
        }

        function updateScreen(title, mode, sub, number = '0') {
            screenLabel.textContent = title;
            screenNumber.textContent = number;
            screenMode.textContent = mode;
            screenSub.textContent = sub;
        }

        function animateStageSwap(callback) {
            stagePanel.classList.add('swap');
            cockpit.classList.add('motion');
            setTimeout(() => {
                callback();
                stagePanel.classList.remove('swap');
                setTimeout(() => cockpit.classList.remove('motion'), 320);
            }, 240);
        }

        function showBrandsStage() {
            dashboardStage = 'brands';
            activeBrand = null;
            updateScreen('Merken', 'LIVE', 'Kies eerst een merk', '3');
            stagePanel.innerHTML = `
                            <div class="stage-head">
                                <div>
                                    <div class="eyebrow">Stap 1</div>
                                    <h3 class="stage-title">Kies een merk</h3>
                                    <p class="stage-text">Na het inschakelen verschijnen eerst de merken. Voorlopig heeft alleen Ninebot al uitgewerkte modelinformatie.</p>
                                </div>
                                <div class="stage-actions">
                                    <button class="ghost-btn" id="goIdentificationTop">IDENTIFICATIE</button>
                                </div>
                            </div>

                            <div class="stage-grid">
                                ${brands.map(brand => `
                                    <div class="brand-card" data-brand="${brand.key}">
                                        <div class="brand-logo ${brand.key}">
                                            <img src="${brandLogos[brand.key]}" alt="${brand.name} logo">
                                        </div>
                                        <div class="brand-name">${brand.name}</div>
                                        <div class="brand-sub">${brand.sub}</div>
                                    </div>
                                `).join('')}
                            </div>
                        `;

            document.getElementById('goIdentificationTop').onclick = () => animateStageSwap(showLockStage);
            stagePanel.querySelectorAll('.brand-card').forEach(card => {
                card.onclick = () => animateStageSwap(() => showModelsStage(card.dataset.brand));
            });
        }

        function showModelsStage(brandKey) {
            dashboardStage = 'models';
            activeBrand = brandKey;
            const brandName = brands.find(b => b.key === brandKey)?.name || brandKey;
            const models = modelsByBrand[brandKey] || [];

            updateScreen('Merk actief', brandName.toUpperCase(), models.length ? 'Kies een model' : 'BINNENKORT BESCHIKBAAR', String(models.length));
            if (brandKey === 'xiaomi' || brandKey === 'niu') {
                stagePanel.innerHTML = `
                    <div class="stage-head">
                        <div>
                            <div class="eyebrow">Stap 2</div>
                            <h3 class="stage-title">${brandName}</h3>
                            <p class="stage-text">BINNENKORT BESCHIKBAAR</p>
                        </div>
                        <div class="stage-actions">
                            <button class="ghost-btn" id="backBrandsA">← Terug naar merken</button>
                        </div>
                    </div>

                    <div class="info-card" style="min-height:220px; display:flex; align-items:center; justify-content:center; text-align:center;">
                        <h4 style="font-size:42px; margin:0;">BINNENKORT BESCHIKBAAR</h4>
                    </div>
                `;

                document.getElementById('backBrandsA').onclick = () => animateStageSwap(showBrandsStage);
                return;
            }

            if (!models.length) {
                stagePanel.innerHTML = `
                    <div class="stage-head">
                        <div>
                            <div class="eyebrow">Stap 2</div>
                            <h3 class="stage-title">${brandName} modellen</h3>
                            <p class="stage-text">Voor dit merk is er momenteel nog geen uitgewerkte modeldata.</p>
                        </div>
                        <div class="stage-actions">
                            <button class="ghost-btn" id="backBrandsA">← Terug naar merken</button>
                            <button class="ghost-btn" id="goIdentificationA">IDENTIFICATIE</button>
                        </div>
                    </div>
                    <div class="info-card">
                        <h4>${brandName}</h4>
                        <p>Momenteel is alleen Ninebot volledig uitgewerkt in jouw project.</p>
                    </div>
                `;
                document.getElementById('backBrandsA').onclick = () => animateStageSwap(showBrandsStage);
                document.getElementById('goIdentificationA').onclick = () => animateStageSwap(showLockStage);
                return;
            }

            stagePanel.innerHTML = `
                <div class="stage-head">
                    <div>
                        <div class="eyebrow">Stap 2</div>
                        <h3 class="stage-title">${brandName} modellen</h3>
                        <p class="stage-text">Kies een model. Daarna ga je door naar de modelpagina met alle info, tabs, foutcodes en reparaties.</p>
                    </div>
                    <div class="stage-actions">
                        <button class="ghost-btn" id="backBrands">← Terug naar merken</button>
                        <button class="ghost-btn" id="goIdentificationModels">IDENTIFICATIE</button>
                    </div>
                </div>

                <div class="model-grid">
                    ${models.map((model, index) => `
                        <div class="model-card" data-model-index="${index}">
                            <div class="model-thumb">
                                <div class="model-thumb-text">${model.thumb}</div>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;

            document.getElementById('backBrands').onclick = () => animateStageSwap(showBrandsStage);
            document.getElementById('goIdentificationModels').onclick = () => animateStageSwap(showLockStage);

            stagePanel.querySelectorAll('.model-card').forEach(card => {
                card.addEventListener('click', () => {
                    selectStep(Number(card.dataset.modelIndex), true);
                });
            });
        }

        function showLockStage() {
            dashboardStage = 'lock';
            updateScreen('Identificatie', 'LOCK?', 'Is de step locked?', '?');
            stagePanel.innerHTML = `
            <div class="stage-head">
                <div>
                    <div class="eyebrow">Identificatie</div>
                    <h3 class="stage-title">LOCK?</h3>
                    <p class="stage-text">Sommige steps zijn gelockt of softwarematig beperkt. Kies hieronder of de step gelockt is.</p>
                </div>
                <div class="stage-actions">
                    <button class="ghost-btn" id="backFromLock">← Terug</button>
                </div>
            </div>

            <div class="choice-grid">
                <div class="choice-big" id="lockYes">
                    <div>
                        <h4>JA</h4>
                        <p>De step is gelockt. Toon de unlock-aanpak en extra info.</p>
                    </div>
                </div>

                <div class="choice-big" id="lockNo">
                    <div>
                        <h4>NEE</h4>
                        <p>De step is niet gelockt. Ga verder met app-identificatie of AI-analyse.</p>
                    </div>
                </div>
            </div>
        `;

            document.getElementById('backFromLock').onclick = () =>
                animateStageSwap(() => activeBrand ? showModelsStage(activeBrand) : showBrandsStage());

            document.getElementById('lockYes').onclick = () => animateStageSwap(showUnlockStage);
            document.getElementById('lockNo').onclick = () => animateStageSwap(showIdentifyChoiceStage);
        }

        function showUnlockStage() {
            dashboardStage = 'unlock';
            updateScreen('Unlock', 'LOCKED', 'Gebruik ScooterHacking', '!');
            stagePanel.innerHTML = `
                            <div class="stage-head">
                                <div>
                                    <div class="eyebrow">Unlock</div>
                                    <h3 class="stage-title">Gebruik ScooterHacking.org</h3>
                                    <p class="stage-text">Als de step gelockt is, kan je via ScooterHacking verdere info bekijken. Werk voorzichtig bij firmware en unlock-methodes.</p>
                                </div>
                                <div class="stage-actions">
                                    <button class="ghost-btn" id="backToLockFromUnlock">← Terug naar LOCK</button>
                                </div>
                            </div>

                            <div class="choice-grid">
                                <div class="info-card">
                                    <h4>Wat doe je hier?</h4>
                                    <p>Controleer eerst welk model en welke firmware je hebt. Kijk daarna of jouw step ondersteund wordt.</p>
                                </div>
                                <div class="info-card">
                                    <h4>Platform</h4>
                                    <p>Gebruik de website hieronder om verder te gaan met unlocken, compatibiliteit en extra technische info.</p>
                                    <div class="stage-actions" style="margin-top:18px;">
                                        <a class="stage-btn" href="https://donate.scooterhacking.org/" target="_blank" rel="noopener noreferrer">Open ScooterHacking.org</a>
                                    </div>
                                </div>
                            </div>
                        `;
            document.getElementById('backToLockFromUnlock').onclick = () => animateStageSwap(showLockStage);
        }

        function showIdentifyChoiceStage() {
            dashboardStage = 'identify-choice';
            updateScreen('Identificatie', 'MODEL', 'App of AI?', '2');

            stagePanel.innerHTML = `
            <div class="stage-head">
                <div>
                    <div class="eyebrow">Identificatie</div>
                    <h3 class="stage-title">Hoe wil je identificeren?</h3>
                    <p class="stage-text">Kies of je het model via de app wil detecteren of via AI-analyse op basis van een foto en zichtbare kenmerken.</p>
                </div>
                <div class="stage-actions">
                    <button class="ghost-btn" id="backToLockFromChoice">← Terug naar LOCK</button>
                </div>
            </div>

            <div class="choice-grid">
                <div class="choice-big" id="identifyAppBtn">
                    <div>
                        <h4>App</h4>
                        <p>Gebruik bluetooth en de officiële Ninebot-app om model- en apparaatgegevens te bekijken.</p>
                    </div>
                </div>

                <div class="choice-big" id="identifyAIBtn">
                    <div>
                        <h4>AI Analyse</h4>
                        <p>Upload een foto, geef zichtbare kenmerken in en laat het systeem berekenen welk Ninebot-model het waarschijnlijk is.</p>
                    </div>
                </div>
            </div>
        `;

            document.getElementById('backToLockFromChoice').onclick = () => animateStageSwap(showLockStage);
            document.getElementById('identifyAppBtn').onclick = () => animateStageSwap(showAppStage);
            document.getElementById('identifyAIBtn').onclick = () => animateStageSwap(showAIStage);
        }

        function showAppStage() {
            dashboardStage = 'identify-app';
            updateScreen('Identificatie', 'APP', 'Gebruik bluetooth', 'A');
            stagePanel.innerHTML = `
            <div class="stage-head">
                <div>
                    <div class="eyebrow">App-identificatie</div>
                    <h3 class="stage-title">Identificeren via de app</h3>
                    <p class="stage-text">Gebruik de officiële Ninebot-app om de step via bluetooth te verbinden en de modelinformatie uit te lezen.</p>
                </div>
                <div class="stage-actions">
                    <button class="ghost-btn" id="backToChoiceFromApp">← Terug</button>
                </div>
            </div>

            <div class="choice-grid">
                <div class="info-card">
                    <h4>Hoe werkt het?</h4>
                    <p>
                        1. Zet bluetooth aan op je smartphone.<br>
                        2. Zet de step aan.<br>
                        3. Open de Ninebot-app.<br>
                        4. Verbind met de step.<br>
                        5. Controleer modelnaam, serienummer en apparaatinformatie in de app.
                    </p>
                </div>

                <div class="info-card">
                    <h4>App-link</h4>
                    <p>Download of open de officiële Ninebot-app via onderstaande knop.</p>
                    <div class="stage-actions" style="margin-top:18px;">
                        <a class="stage-btn" href="https://app.ninebot.com/downloads/v5/en.html" target="_blank" rel="noopener noreferrer">Open Ninebot app</a>
                    </div>
                    <div class="stage-actions" style="margin-top:12px;">
                        <button class="ghost-btn" id="appNotWorkingBtn">Werkt de app niet? Gebruik AI Analyse</button>
                    </div>
                </div>
            </div>
        `;

            document.getElementById('backToChoiceFromApp').onclick = () => animateStageSwap(showIdentifyChoiceStage);
            document.getElementById('appNotWorkingBtn').onclick = () => animateStageSwap(showAIStage);
        }

        function humanizeValue(value, mapping) {
            return mapping[value] || 'Niet zeker';
        }

        function updateAIPreview() {
            const preview = document.getElementById('aiPreviewBox');
            const fileNameEl = document.getElementById('aiFileName');
            if (!preview || !fileNameEl) return;

            fileNameEl.textContent = aiPhotoName || 'Geen foto geselecteerd';

            if (aiPhotoDataUrl) {
                preview.innerHTML = `
                <div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;overflow:hidden;background:#f2f2f2;">
                    <img
                        src="${aiPhotoDataUrl}"
                        alt="Preview step"
                        style="max-width:100%;max-height:100%;width:auto;height:auto;object-fit:contain;object-position:center;display:block;"
                    >
                </div>
            `;
            } else {
                preview.innerHTML = `
                <div style="width:100%;height:100%;display:grid;place-items:center;text-align:center;color:#9fb2d2;padding:20px;background:rgba(255,255,255,.02);">
                    <div>
                        <div style="font-size:42px;margin-bottom:10px;">📷</div>
                        <div style="font-weight:800;margin-bottom:6px;">Nog geen foto gekozen</div>
                        <div style="font-size:14px;">Upload een foto van de step en voeg extra kenmerken toe.</div>
                    </div>
                </div>
            `;
            }
        }

        function resetAIStage() {
            aiPhotoName = '';
            aiPhotoDataUrl = '';
            aiDescription = '';
            aiSuspension = 'unknown';
            aiRedAccents = 'unknown';
            aiFrameSize = 'unknown';
            aiWheelSize = 'unknown';
            aiStemShape = 'unknown';
            aiDeckType = 'unknown';
            showAIStage();
        }

        function detectNinebotModel({
            text,
            hasSuspension,
            hasRedAccents,
            frameSize,
            photoName,
            wheelSize,
            stemShape,
            deckType
        }) {
            const normalized = ((text || '') + ' ' + (photoName || '')).toLowerCase();

            const score = {
                g30: 0,
                d18d38: 0,
                maxg2: 0,
                f65i: 0,
                f40: 0
            };

            if (hasSuspension === 'yes') {
                score.maxg2 += 5;
                score.g30 += 1;
            }

            if (hasRedAccents === 'yes') {
                score.d18d38 += 3;
                score.f40 += 1;
            }

            if (frameSize === 'large') {
                score.g30 += 3;
                score.maxg2 += 5;
                score.f65i += 3;
            }

            if (frameSize === 'compact') {
                score.d18d38 += 3;
                score.f40 += 3;
            }

            if (wheelSize === 'large10') {
                score.g30 += 2;
                score.maxg2 += 3;
                score.f65i += 2;
                score.f40 += 2;
                score.d18d38 += 2;
            }

            if (stemShape === 'thick') {
                score.g30 += 2;
                score.maxg2 += 4;
                score.f65i += 2;
            }

            if (stemShape === 'slim') {
                score.f40 += 3;
                score.d18d38 += 2;
            }

            if (deckType === 'wide') {
                score.g30 += 3;
                score.maxg2 += 4;
                score.f65i += 2;
            }

            if (deckType === 'narrow') {
                score.d18d38 += 3;
                score.f40 += 2;
            }

            const keywordMap = [
                { keys: ['g30', 'max g30'], model: 'g30', points: 8 },
                { keys: ['d18', 'd38', 'd-serie', 'd series'], model: 'd18d38', points: 8 },
                { keys: ['g2', 'max g2'], model: 'maxg2', points: 8 },
                { keys: ['f65', 'f65i'], model: 'f65i', points: 8 },
                { keys: ['f40'], model: 'f40', points: 8 },
                { keys: ['vering', 'suspension'], model: 'maxg2', points: 5 },
                { keys: ['grote step', 'groot frame', 'breed deck', 'robuust'], model: 'g30', points: 3 },
                { keys: ['premium', 'comfort'], model: 'maxg2', points: 3 },
                { keys: ['lange rit', 'groot bereik'], model: 'f65i', points: 3 },
                { keys: ['compact', 'licht model'], model: 'f40', points: 3 },
                { keys: ['instapmodel', 'eenvoudig'], model: 'd18d38', points: 2 }
            ];

            keywordMap.forEach(({ keys, model, points }) => {
                if (keys.some(key => normalized.includes(key))) score[model] += points;
            });

            const ranked = Object.entries(score).sort((a, b) => b[1] - a[1]);
            const [winner, winnerScore] = ranked[0];
            const [, runnerUpScore] = ranked[1];

            const modelInfo = {
                g30: { name: 'Ninebot MAX G30', short: 'G30', code: 'G30', summary: 'Waarschijnlijke match voor een robuust MAX-model.', why: ['Brede en stevige MAX-opbouw.', 'Typisch groot deck en robuust frame.', 'Vaak herkenbaar als klassiek MAX-model.'] },
                d18d38: { name: 'Ninebot D18 / D38', short: 'D18-D38', code: 'D18-D38', summary: 'Waarschijnlijke match voor een step uit de D-serie.', why: ['Slankere D-serie opbouw.', 'Minder premium dan MAX-modellen.', 'Past bij eenvoudiger commuter-design.'] },
                maxg2: { name: 'Ninebot MAX G2', short: 'MAX G2', code: 'MAX G2', summary: 'Waarschijnlijke match voor de MAX G2 met premium frame en vering.', why: ['Sterke kans door aanwezigheid van vering.', 'Groter premium frame.', 'Meer comfort- en commuter-uitstraling.'] },
                f65i: { name: 'Ninebot F65I', short: 'F65I', code: 'F65I', summary: 'Waarschijnlijke match voor een grotere F-serie step.', why: ['Grotere F-serie commuter-step.', 'Langer en steviger frame dan F40.', 'Past bij bereikgerichte F-serie uitstraling.'] },
                f40: { name: 'Ninebot F40', short: 'F40', code: 'F40', summary: 'Waarschijnlijke match voor een compactere F-serie step.', why: ['Slankere F-serie vorm.', 'Lichter commuter-model.', 'Past minder bij G-serie en MAX-opbouw.'] }
            };

            const base = modelInfo[winner];
            const confidence = Math.max(60, Math.min(97, 68 + (winnerScore - runnerUpScore) * 6 + winnerScore * 2));

            return {
                key: winner,
                name: base.name,
                short: base.short,
                code: base.code,
                summary: base.summary,
                confidence,
                why: base.why
            };
        }

        function showAIStage() {
            dashboardStage = 'identify-ai';
            updateScreen('Identificatie', 'AI', 'Foto-analyse', 'AI');

            stagePanel.innerHTML = `
            <div class="stage-head">
                <div>
                    <div class="eyebrow">AI Analyse</div>
                    <h3 class="stage-title">Herken de step via foto</h3>
                    <p class="stage-text">Upload een foto en vul zichtbare kenmerken in. Daarna berekent het systeem welk Ninebot-model het meest waarschijnlijk is.</p>
                </div>
                <div class="stage-actions">
                    <button class="ghost-btn" id="backToChoiceFromAI">← Terug</button>
                    <button class="ghost-btn" id="resetAIStageBtn">Reset</button>
                </div>
            </div>

            <div class="choice-grid">
                <div class="info-card">
                    <h4>Foto uploaden</h4>
                    <label class="ghost-btn" style="margin-top:10px; cursor:pointer;">
                        Kies foto
                        <input type="file" id="aiPhotoInput" accept="image/*" style="display:none;">
                    </label>

                    <div id="aiPreviewBox" style="margin-top:16px;height:320px;border-radius:18px;border:1px solid rgba(255,255,255,.08);background:rgba(255,255,255,.03);display:grid;place-items:center;padding:0;overflow:hidden;"></div>
                    <p style="margin-top:12px;"><strong>Bestand:</strong> <span id="aiFileName">Geen foto geselecteerd</span></p>
                </div>

                <div class="info-card">
                    <h4>Extra kenmerken</h4>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Beschrijving</label>
                        <textarea id="aiDescriptionInput" style="width:100%;min-height:110px;background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.08);border-radius:14px;padding:12px;color:white;resize:vertical;" placeholder="Bijvoorbeeld: grote step, stevig frame, geen rode accenten, modern model...">${aiDescription}</textarea>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Vering zichtbaar?</label>
                        <select id="aiSuspensionInput" class="ai-select">
                            <option value="unknown" ${aiSuspension === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="yes" ${aiSuspension === 'yes' ? 'selected' : ''}>Ja</option>
                            <option value="no" ${aiSuspension === 'no' ? 'selected' : ''}>Nee</option>
                        </select>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Rode accenten?</label>
                        <select id="aiRedInput" class="ai-select">
                            <option value="unknown" ${aiRedAccents === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="yes" ${aiRedAccents === 'yes' ? 'selected' : ''}>Ja</option>
                            <option value="no" ${aiRedAccents === 'no' ? 'selected' : ''}>Nee</option>
                        </select>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Framegrootte</label>
                        <select id="aiFrameInput" class="ai-select">
                            <option value="unknown" ${aiFrameSize === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="large" ${aiFrameSize === 'large' ? 'selected' : ''}>Groot / robuust</option>
                            <option value="compact" ${aiFrameSize === 'compact' ? 'selected' : ''}>Compact / slank</option>
                        </select>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Wielgrootte</label>
                        <select id="aiWheelInput" class="ai-select">
                            <option value="unknown" ${aiWheelSize === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="large10" ${aiWheelSize === 'large10' ? 'selected' : ''}>Groot / 10 inch-achtig</option>
                            <option value="small" ${aiWheelSize === 'small' ? 'selected' : ''}>Kleiner wiel</option>
                        </select>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Stuurstang / framevorm</label>
                        <select id="aiStemInput" class="ai-select">
                            <option value="unknown" ${aiStemShape === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="thick" ${aiStemShape === 'thick' ? 'selected' : ''}>Dik / stevig</option>
                            <option value="slim" ${aiStemShape === 'slim' ? 'selected' : ''}>Slank / smal</option>
                        </select>
                    </div>

                    <div style="margin-top:12px;">
                        <label style="display:block;font-weight:800;margin-bottom:6px;">Decktype</label>
                        <select id="aiDeckInput" class="ai-select">
                            <option value="unknown" ${aiDeckType === 'unknown' ? 'selected' : ''}>Niet zeker</option>
                            <option value="wide" ${aiDeckType === 'wide' ? 'selected' : ''}>Breed deck</option>
                            <option value="narrow" ${aiDeckType === 'narrow' ? 'selected' : ''}>Smaller deck</option>
                        </select>
                    </div>

                    <div class="stage-actions" style="margin-top:16px;">
                        <button class="stage-btn" id="runAIAnalysisBtn">Start AI-analyse</button>
                    </div>
                </div>
            </div>
        `;

            document.getElementById('backToChoiceFromAI').onclick = () => animateStageSwap(showIdentifyChoiceStage);
            document.getElementById('resetAIStageBtn').onclick = () => resetAIStage();

            const photoInput = document.getElementById('aiPhotoInput');
            photoInput.onchange = (e) => {
                const file = e.target.files?.[0];
                if (!file) return;
                aiPhotoName = file.name;
                const reader = new FileReader();
                reader.onload = () => {
                    aiPhotoDataUrl = reader.result;
                    updateAIPreview();
                };
                reader.readAsDataURL(file);
            };

            document.getElementById('aiDescriptionInput').oninput = (e) => { aiDescription = e.target.value; };
            document.getElementById('aiSuspensionInput').onchange = (e) => { aiSuspension = e.target.value; };
            document.getElementById('aiRedInput').onchange = (e) => { aiRedAccents = e.target.value; };
            document.getElementById('aiFrameInput').onchange = (e) => { aiFrameSize = e.target.value; };
            document.getElementById('aiWheelInput').onchange = (e) => { aiWheelSize = e.target.value; };
            document.getElementById('aiStemInput').onchange = (e) => { aiStemShape = e.target.value; };
            document.getElementById('aiDeckInput').onchange = (e) => { aiDeckType = e.target.value; };

            document.getElementById('runAIAnalysisBtn').onclick = () => animateStageSwap(showAIResultStage);

            updateAIPreview();
        }

        function showAIResultStage() {
            const result = detectNinebotModel({
                text: aiDescription,
                hasSuspension: aiSuspension,
                hasRedAccents: aiRedAccents,
                frameSize: aiFrameSize,
                photoName: aiPhotoName,
                wheelSize: aiWheelSize,
                stemShape: aiStemShape,
                deckType: aiDeckType
            });

            updateScreen('Analyse', result.short, 'Model gevonden', String(result.confidence));

            stagePanel.innerHTML = `
            <div class="stage-head">
                <div>
                    <div class="eyebrow">AI resultaat</div>
                    <h3 class="stage-title">${result.name}</h3>
                    <p class="stage-text">${result.summary}</p>
                </div>
                <div class="stage-actions">
                    <button class="ghost-btn" id="backToAIInput">← Terug</button>
                    <button class="ghost-btn" id="resetAIResultBtn">Reset</button>
                </div>
            </div>

            <div class="choice-grid">
                <div class="info-card">
                    <h4>Confidence</h4>
                    <p style="font-size:42px;font-weight:900;margin-bottom:12px;">${result.confidence}%</p>
                    <p>${result.confidence >= 85 ? 'Hoge betrouwbaarheid.' : result.confidence >= 72 ? 'Goede waarschijnlijkheid.' : 'Voorzichtige match — extra controle aanbevolen.'}</p>
                </div>

                <div class="info-card">
                    <h4>Waarom dit model?</h4>
                    <ul style="margin:0;padding-left:20px;color:#d8e4fb;line-height:1.8;">
                        ${result.why.map(item => `<li>${item}</li>`).join('')}
                    </ul>
                </div>
            </div>

            <div class="info-card" style="margin-top:16px;">
                <h4>Gebruikte input</h4>
                <p><strong>Bestand:</strong> ${aiPhotoName || 'Geen foto'}</p>
                <p><strong>Vering:</strong> ${humanizeValue(aiSuspension, { yes: 'Ja', no: 'Nee', unknown: 'Niet zeker' })}</p>
                <p><strong>Rode accenten:</strong> ${humanizeValue(aiRedAccents, { yes: 'Ja', no: 'Nee', unknown: 'Niet zeker' })}</p>
                <p><strong>Frame:</strong> ${humanizeValue(aiFrameSize, { large: 'Groot', compact: 'Compact', unknown: 'Niet zeker' })}</p>
                <p><strong>Wielen:</strong> ${humanizeValue(aiWheelSize, { large10: 'Groot / 10 inch-achtig', small: 'Kleiner wiel', unknown: 'Niet zeker' })}</p>
                <p><strong>Stuurstang:</strong> ${humanizeValue(aiStemShape, { thick: 'Dik / stevig', slim: 'Slank / smal', unknown: 'Niet zeker' })}</p>
                <p><strong>Deck:</strong> ${humanizeValue(aiDeckType, { wide: 'Breed', narrow: 'Smaller', unknown: 'Niet zeker' })}</p>
                <p style="margin-top:10px;"><strong>Beschrijving:</strong> ${aiDescription || 'Geen extra beschrijving toegevoegd.'}</p>

                <div class="stage-actions" style="margin-top:16px;">
                    <button class="stage-btn" id="chooseDetectedModelBtn">Open modelpagina</button>
                </div>
            </div>
        `;

            document.getElementById('backToAIInput').onclick = () => animateStageSwap(showAIStage);
            document.getElementById('resetAIResultBtn').onclick = () => resetAIStage();
            document.getElementById('chooseDetectedModelBtn').onclick = () => {
                if (!activeBrand) activeBrand = 'ninebot';
                const brandModels = modelsByBrand['ninebot'];
                const detectedIndex = brandModels.findIndex(model => model.code === result.code);

                if (detectedIndex >= 0) {
                    selectStep(detectedIndex, true);
                } else {
                    animateStageSwap(() => showModelsStage('ninebot'));
                }
            };
        }

        function renderLinks(items) {
            if (!items || !items.length) return '<div class="empty">Nog geen links geladen.</div>';
            return `<div class="links-row">${items.map(item => `<a class="link-btn" href="${item.href}" target="_blank" rel="noopener noreferrer">${item.label}</a>`).join('')}</div>`;
        }

        function renderOverview(step) {
            const d = step.data?.overview;
            if (!d) return `<div class="empty">Nog geen overzicht beschikbaar.</div>`;

            return `
                            <div class="hero-card">
                                <div class="hero-image">
                                    <img src="${step.image}" alt="${step.name}">
                                </div>

                                <div class="hero-copy">
                                    <h4>${step.name}</h4>
                                    <p>De ${step.name} is een ${d.category.toLowerCase()} uit de <strong>${d.series}</strong> van Segway-Ninebot.</p>

                                    <div class="links-row" style="margin-top:16px; margin-bottom:14px;">
                                        ${(step.data.links || []).slice(0, 2).map(item => `
                                            <a class="link-btn" href="${item.href}" target="_blank" rel="noopener noreferrer">${item.label}</a>
                                        `).join('')}
                                    </div>
                                </div>
                            </div>

                            <div class="stats">
                                <div class="stat">
                                    <div class="k">Model</div>
                                    <div class="v">${d.model}</div>
                                </div>
                                <div class="stat">
                                    <div class="k">Merk</div>
                                    <div class="v">${d.brand}</div>
                                </div>
                                <div class="stat">
                                    <div class="k">Gewicht</div>
                                    <div class="v">${d.weight}</div>
                                </div>
                                <div class="stat">
                                    <div class="k">Max. draaggewicht</div>
                                    <div class="v">${d.maxLoad}</div>
                                </div>
                            </div>

                            <div class="grid">
                                <div class="card">
                                    <h5>Accu & bereik</h5>
                                    <p>
                                        <strong>Accu:</strong> ${d.battery}<br>
                                        <strong>Bereik:</strong> ${d.range}<br>
                                        <strong>Laadtijd:</strong> ${d.charge}<br>
                                        <strong>App:</strong> ${d.app}
                                    </p>
                                </div>

                                <div class="card">
                                    <h5>Motor & prestaties</h5>
                                    <p>
                                        <strong>Motor:</strong> ${d.motor}<br>
                                        <strong>Snelheid:</strong> ${d.speed}<br>
                                        <strong>Hellingsvermogen:</strong> ${d.climb}<br>
                                        <strong>Aandrijving:</strong> ${d.drive}
                                    </p>
                                </div>

                                <div class="card">
                                    <h5>Wielen & remmen</h5>
                                    <p>
                                        <strong>Wielen:</strong> ${d.wheels}<br>
                                        <strong>Remmen:</strong> ${d.brakes}<br>
                                        <strong>Verlichting:</strong> ${d.lights}
                                    </p>
                                </div>

                                <div class="card">
                                    <h5>Dashboard & afmetingen</h5>
                                    <p>
                                        <strong>Display:</strong> ${d.display}<br>
                                        <strong>Rijmodi:</strong> ${d.rideModes}<br>
                                        <strong>Open:</strong> ${d.sizeOpen}<br>
                                        <strong>Dicht:</strong> ${d.sizeClosed}
                                    </p>
                                </div>
                            </div>

                            <div class="wide">
                                <h5>Technische kerninfo</h5>
                                <ul>
                                    ${(step.data.theory || []).map(item => `<li>${item}</li>`).join('')}
                                </ul>
                            </div>
                        `;
        }

        function renderSafety(step) {
            if (!step.data) return `<div class="empty">Nog geen veiligheidsinfo geladen.</div>`;
            return `
                            <div class="big-center-card">
                                <h5>Veiligheid / Voorbereiding</h5>
                                <p>Open hier de volledige handleiding met alle veiligheids- en voorbereidingsstappen voor dit model.</p>
                                <a class="big-doc-btn" href="${step.data.safetyLink}" target="_blank" rel="noopener noreferrer">
                                    📄 Veiligheid / Voorbereiding
                                </a>
                            </div>
                        `;
        }

        function renderDemontage(step) {
            if (!step.data) return `<div class="empty">Nog geen demontagefiche geladen voor ${step.name}.</div>`;
            return `
                            <div class="big-center-card">
                                <h5>Handleiding / Demontage</h5>
                                <p>Open hier de volledige handleiding van de demontage voor dit model.</p>
                                <a class="big-doc-btn" href="${step.data.demontageLink}" target="_blank" rel="noopener noreferrer">
                                    📄 Handleiding / Demontage
                                </a>
                            </div>
                        `;
        }

        function renderParts(step) {
            if (!step.data) return `<div class="empty">Nog geen onderdelenlijst geladen.</div>`;

            return `
                <div class="parts-main-actions">
                    <a class="parts-main-btn" href="${step.data.partsGuideLink}" target="_blank" rel="noopener noreferrer">
                        📑 Onderdelen
                    </a>

                    <a class="parts-main-btn" href="https://docs.google.com/spreadsheets/d/1fdFT0VOUxwT593_fVUETPX4iiZhn40KvdwuPQKK94W8/edit?usp=drive_link" target="_blank" rel="noopener noreferrer">
                        💶 Reparatiekosten
                    </a>
                </div>

                <div class="parts-grid">
                    ${step.data.parts.map(part => `
                        <div class="part-card">
                            <div class="part-top">
                                <div class="part-name">${part.name}</div>
                                <div class="tag">Stock: ${part.stock}</div>
                            </div>

                            <div class="part-description">${part.description}</div>

                            <div class="mini-actions">
                                <button class="mini-btn js-open-part" data-part="${part.key}" data-name="${part.name}" data-description="${part.description}" type="button">
                                    ${part.name}
                                </button>

                                <a class="mini-btn test-btn" href="${part.testLink}" target="_blank" rel="noopener noreferrer">Test</a>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderErrors(step) {
            if (!step.data) return `<div class="empty">Nog geen foutcode-overzicht geladen.</div>`;

            const noPlanCodes = ['17', '20', '28', '29', '42', '43', '44', '45', '46', '47', '48', '49'];

            return `
            <div class="wide section-top-block">
                <div style="display:flex;justify-content:space-between;align-items:center;gap:16px;flex-wrap:wrap;">
                    <div>
                        <h5>Foutcodes & diagnose</h5>
                        <p>Hieronder staan de foutcodes met uitleg, actie, technische achtergrond, een link en een visueel stappenplan.</p>
                    </div>

                    <a
                        class="parts-main-btn"
                        href="https://support.levyelectric.com/articles/13055091563540-Segway-Max-Error-Codes"
                        target="_blank"
                        rel="noopener noreferrer"
                    >
                        Website stappenplannen
                    </a>
                </div>
            </div>

            <div class="parts-grid">
                ${step.data.errorCodes.map(err => `
                    <div class="part-card">
                        <div class="part-top">
                            <div class="part-name">Foutcode ${err[0]}</div>
                            <div class="tag">Diagnose</div>
                        </div>

                        <div style="color:#d8e4fb;line-height:1.7;font-size:14px;">
                            <strong>Probleem:</strong> ${err[1]}<br>
                            <strong>Actie:</strong> ${err[2]}<br>
                            <strong>Technisch:</strong> ${err[3]}
                        </div>

                        <div class="mini-actions" style="margin-top:12px;">
                            <a class="mini-btn" href="${err[4]}" target="_blank" rel="noopener noreferrer">
                                Open foutcode
                            </a>

                            ${noPlanCodes.includes(String(err[0]))
                    ? ''
                    : `<button class="mini-btn test-btn js-open-error-plan" type="button" data-code="${err[0]}">Stappenplan</button>`
                }
                        </div>
                    </div>
                `).join('')}
            </div>
        `;
        }

        function renderSuppliers(step) {
            if (!step.data) return `<div class="empty">Nog geen leveranciersinfo geladen.</div>`;

            return `
                <div class="big-center-card">
                    <h5>Leveranciers</h5>
                    <p>Open hier het volledige leveranciersbestand voor onderdelen, prijzen en beschikbaarheid.</p>
                    <a class="big-doc-btn" href="https://studentkdg-my.sharepoint.com/:x:/r/personal/denny_djanaloev_student_kdg_be/Documents/Ninebot_Leveranciers8.xlsx?d=w6439ae602611481aab1ff60a65b1f7f1&csf=1&web=1&e=xGY9te" target="_blank" rel="noopener noreferrer">
                        📄 Leveranciers
                    </a>
                </div>
            `;
        }

        function renderLinksTab(step) {
            if (!step.data) return `<div class="empty">Nog geen links geladen.</div>`;
            return `
                            <div class="resource-grid">
                                <div class="link-group">
                                    <h5>Bronnen & documenten</h5>
                                    ${renderLinks(step.data.links)}
                                </div>
                                <div class="link-group">
                                    <h5>Gebruik in dit dashboard</h5>
                                    <p>Voor ${step.name} zijn overzicht, onderdelen, foutcodes en reparatieblokken gekoppeld.</p>
                                </div>
                            </div>
                        `;
        }

        function attachPartButtons() {
            document.querySelectorAll('.js-open-part').forEach(btn => {
                btn.addEventListener('click', () => {
                    openPartModal(btn.dataset.part, btn.dataset.name, btn.dataset.description);
                });
            });
        }

        function attachErrorPlanButtons() {
            document.querySelectorAll('.js-open-error-plan').forEach(btn => {
                btn.addEventListener('click', () => {
                    openErrorPlanModal(btn.dataset.code);
                });
            });
        }

        function renderTab() {
            const step = getActiveStep();
            if (!step) {
                contentPanel.innerHTML = `<div class="empty">Kies eerst een model op het dashboard.</div>`;
                return;
            }

            if (activeTab === 'overview') contentPanel.innerHTML = renderOverview(step);
            if (activeTab === 'safety') contentPanel.innerHTML = renderSafety(step);
            if (activeTab === 'demontage') contentPanel.innerHTML = renderDemontage(step);
            if (activeTab === 'parts') {
                contentPanel.innerHTML = renderParts(step);
                attachPartButtons();
            }
            if (activeTab === 'errors') {
                contentPanel.innerHTML = renderErrors(step);
                attachErrorPlanButtons();
            }
            if (activeTab === 'suppliers') contentPanel.innerHTML = renderSuppliers(step);
            if (activeTab === 'links') contentPanel.innerHTML = renderLinksTab(step);
        }

        function selectStep(index, openDetail = true) {
            activeIndex = index;
            const step = getActiveStep();
            if (!step) return;

            cockpit.classList.add('motion');
            setTimeout(() => cockpit.classList.remove('motion'), 700);

            updateScreen('Actief model', step.code, step.name, '0');
            titleModel.textContent = step.name;
            titleDesc.textContent = '';
            modelStatus.textContent = step.status;
            renderTab();

            if (openDetail) switchToContent();
        }

        function openPartModal(key, name, description) {
            const step = getActiveStep();
            const modelCode = step?.code || '';
            const modelImages = PART_IMAGES[modelCode] || {};
            const fallbackImages = PART_IMAGES.fallback || {};

            partModalTitle.textContent = name;
            partModalSub.textContent = description;
            partModalImage.src = modelImages[key] || fallbackImages[key] || fallbackImages.controller;
            partModalImage.alt = name;
            partModal.classList.add('show');
            document.body.style.overflow = 'hidden';
        }

        function closePartOverlay() {
            partModal.classList.remove('show');
            document.body.style.overflow = '';
        }

        function openErrorPlanModal(code) {
            const imagePath = ERROR_STEP_IMAGES[code];

            if (!imagePath) {
                return;
            }

            errorPlanImage.onerror = function () {
                errorPlanModal.classList.remove('show');
                document.body.style.overflow = '';
                errorPlanImage.onerror = null;
            };

            errorPlanImage.src = imagePath;
            errorPlanImage.alt = `Stappenplan foutcode ${code}`;
            errorPlanModal.classList.add('show');
            document.body.style.overflow = 'hidden';
        }

        function closeErrorPlanOverlay() {
            errorPlanModal.classList.remove('show');
            errorPlanImage.src = '';
            document.body.style.overflow = '';
        }

        function switchToContent() {
            if (currentView === 'content') return;
            currentView = 'content';
            dashboardView.classList.add('exit');

            setTimeout(() => {
                dashboardView.classList.add('hidden');
                dashboardView.classList.remove('exit');
                contentView.classList.remove('hidden');
                contentView.classList.add('enter');
                setTimeout(() => contentView.classList.remove('enter'), 820);
            }, 620);
        }

        function switchToDashboard() {
            if (currentView === 'dashboard') return;
            currentView = 'dashboard';
            contentView.classList.add('exit');

            setTimeout(() => {
                contentView.classList.add('hidden');
                contentView.classList.remove('exit');
                dashboardView.classList.remove('hidden');
                dashboardView.classList.add('enter');
                setTimeout(() => dashboardView.classList.remove('enter'), 820);
            }, 520);
        }

        async function powerOn() {
            if (poweredOn) return;
            poweredOn = true;

            powerButton.classList.add('on');
            bootBox.classList.add('show');
            systemState.textContent = 'Systeem opstarten...';
            updateScreen('Booting', 'ON', 'Dashboard start op', '...');

            for (let i = 0; i <= 100; i += 10) {
                bootFill.style.width = i + '%';
                await sleep(120);
            }

            await sleep(250);
            bootBox.classList.remove('show');
            stagePanel.classList.remove('hidden');
            systemState.textContent = 'Systeem actief';
            showBrandsStage();
        }

        document.addEventListener('DOMContentLoaded', () => {
            initRefs();
            setTabsState();

            powerButton.addEventListener('click', powerOn);

            backToDashboard.addEventListener('click', () => {
                switchToDashboard();
                setTimeout(() => {
                    if (activeBrand) showModelsStage(activeBrand);
                    else showBrandsStage();
                }, 540);
            });

            tabButtons.forEach(btn => {
                btn.addEventListener('click', () => {
                    activeTab = btn.dataset.tab;
                    setTabsState();
                    renderTab();
                });
            });

            if (closePartModal) {
                closePartModal.addEventListener('click', closePartOverlay);
            }

            if (partModal) {
                partModal.addEventListener('click', (e) => {
                    if (e.target === partModal) closePartOverlay();
                });
            }

            if (closeErrorPlanModal) {
                closeErrorPlanModal.addEventListener('click', closeErrorPlanOverlay);
            }

            if (errorPlanModal) {
                errorPlanModal.addEventListener('click', (e) => {
                    if (e.target === errorPlanModal) closeErrorPlanOverlay();
                });
            }

            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    closePartOverlay();
                    closeErrorPlanOverlay();
                }
            });
        });
    </script>
</body>
</html>
