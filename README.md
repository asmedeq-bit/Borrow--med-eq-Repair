<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>ระบบยืมคืนเครื่องมือแพทย์ — Supabase</title>
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&family=IBM+Plex+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#0d1117;--s1:#161b22;--s2:#1f2937;--bd:#30363d;--ac:#00c8ff;--ac2:#7c3aed;--ok:#10b981;--wn:#f59e0b;--er:#ef4444;--tx:#e6edf3;--mu:#8b949e;--fn:'Sarabun',sans-serif;--mo:'IBM Plex Mono',monospace}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--fn);background:var(--bg);color:var(--tx);min-height:100vh;font-size:14px}
#cfgBanner{background:rgba(239,68,68,.12);border-bottom:1px solid var(--er);padding:8px 16px;font-size:12px;color:var(--er);display:none;text-align:center}
#cfgBanner.on{display:block}
.nav{background:var(--s1);border-bottom:1px solid var(--bd);display:flex;align-items:center;position:sticky;top:0;z-index:100;overflow:hidden}
.nav-brand{padding:10px 13px;font-weight:700;font-size:12px;color:var(--ac);border-right:1px solid var(--bd);white-space:nowrap;flex-shrink:0}
.nav-tabs{display:flex;overflow-x:auto;flex:1}
.nt{padding:11px 12px;font-size:12px;font-weight:600;cursor:pointer;border-right:1px solid var(--bd);white-space:nowrap;color:var(--mu);transition:all .2s;border-bottom:3px solid transparent}
.nt:hover{color:var(--tx);background:var(--s2)}.nt.on{color:var(--ac);border-bottom-color:var(--ac);background:var(--s2)}
.nt.admin-tab{color:#f59e0b}.nt.admin-tab.on{color:#f59e0b;border-bottom-color:#f59e0b}
.pg{display:none;padding:14px;max-width:1500px;margin:0 auto}.pg.on{display:block}
.card{background:var(--s1);border:1px solid var(--bd);border-radius:8px;padding:13px;margin-bottom:11px}
.ctitle{font-size:12px;font-weight:700;color:var(--ac);text-transform:uppercase;letter-spacing:.8px;margin-bottom:10px;display:flex;align-items:center;gap:6px}
.fg{display:flex;flex-direction:column;gap:3px}
.fgrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:9px}
label{font-size:11px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px}
input,select,textarea{background:var(--bg);border:1px solid var(--bd);border-radius:5px;padding:7px 9px;color:var(--tx);font-family:var(--fn);font-size:13px;transition:border .15s;width:100%}
input:focus,select:focus,textarea:focus{outline:none;border-color:var(--ac)}
input[type=date]{position:relative;padding-right:36px;cursor:pointer;color-scheme:dark}
input[type=date]::-webkit-calendar-picker-indicator{position:absolute;right:8px;top:50%;transform:translateY(-50%);width:20px;height:20px;cursor:pointer;opacity:1;filter:invert(1) sepia(1) saturate(5) hue-rotate(175deg) brightness(1.2);background-color:transparent}
select option{background:var(--s1)}
.ssw{position:relative}
.ssl{position:absolute;top:100%;left:0;right:0;background:var(--s1);border:1px solid var(--ac);border-radius:5px;max-height:175px;overflow-y:auto;z-index:200;display:none}
.ssl.on{display:block}
.ssi{padding:7px 10px;cursor:pointer;font-size:13px;display:flex;gap:7px;align-items:center}
.ssi:hover{background:var(--s2)}
.seld{margin-top:4px;background:rgba(0,200,255,.06);border:1px solid rgba(0,200,255,.2);padding:5px 8px;border-radius:5px;font-size:12px;display:none}
.btn{padding:6px 13px;border:none;border-radius:5px;font-family:var(--fn);font-size:12px;font-weight:600;cursor:pointer;transition:all .2s;display:inline-flex;align-items:center;gap:4px}
.bp{background:var(--ac);color:#000}.bp:hover{opacity:.85}
.bg{background:transparent;border:1px solid var(--bd);color:var(--tx)}.bg:hover{border-color:var(--ac);color:var(--ac)}
.bok{background:var(--ok);color:#fff}.bok:hover{opacity:.85}
.bwn{background:var(--wn);color:#000}.bwn:hover{opacity:.85}
.ber{background:var(--er);color:#fff}.ber:hover{opacity:.85}
.badm{background:#f59e0b;color:#000}.badm:hover{opacity:.85}
.sm{padding:4px 9px;font-size:11px}
.bgrp{display:flex;gap:6px;flex-wrap:wrap;margin-top:9px}
.badge{display:inline-flex;align-items:center;padding:2px 7px;border-radius:12px;font-size:11px;font-weight:700}
.ba{background:rgba(16,185,129,.15);color:var(--ok);border:1px solid rgba(16,185,129,.3)}
.bp2{background:rgba(245,158,11,.15);color:var(--wn);border:1px solid rgba(245,158,11,.3)}
.br2{background:rgba(139,148,158,.15);color:var(--mu);border:1px solid rgba(139,148,158,.3)}
.bc2{background:rgba(239,68,68,.15);color:var(--er);border:1px solid rgba(239,68,68,.3)}
.bap{background:rgba(245,158,11,.12);color:#f59e0b;border:1px solid rgba(245,158,11,.4);font-size:10px}
.btr{background:rgba(124,58,237,.12);color:#a78bfa;border:1px solid rgba(124,58,237,.35);font-size:10px}
.dtw{overflow-x:auto}
.dt{width:100%;border-collapse:collapse;font-size:13px}
.dt th{background:var(--s2);color:var(--mu);font-size:10px;text-transform:uppercase;letter-spacing:.4px;padding:7px 9px;border-bottom:1px solid var(--bd);text-align:left;white-space:nowrap}
.dt td{padding:7px 9px;border-bottom:1px solid var(--bd);vertical-align:middle}
.dt tr:hover td{background:rgba(255,255,255,.013)}
.abt{padding:3px 7px;font-size:11px;border-radius:4px;border:1px solid var(--bd);background:transparent;color:var(--tx);cursor:pointer;font-family:var(--fn);transition:all .15s;white-space:nowrap}
.abt:hover{border-color:var(--ac);color:var(--ac)}
.loan-hdr{background:rgba(0,200,255,.055);border-left:3px solid var(--ac)}
.loan-hdr td{padding:8px 10px;border-bottom:1px solid rgba(0,200,255,.15)}
.item-row{border-left:3px solid transparent}
.item-row td{padding:5px 9px;border-bottom:1px solid rgba(48,54,61,.5);background:rgba(255,255,255,.006)}
.item-row:last-child td{border-bottom:2px solid var(--bd)}
.transfer-trail{display:flex;align-items:center;flex-wrap:wrap;gap:4px;margin-top:5px;font-size:11px}
.tr-label{color:var(--mu);margin-right:2px;font-size:11px}
.tr-arr{color:var(--mu);font-size:10px}
.tr-chip{background:var(--s2);border:1px solid var(--bd);border-radius:4px;padding:1px 7px;color:var(--mu);font-size:11px}
.tr-chip-cur{background:rgba(0,200,255,.12);border:1px solid rgba(0,200,255,.4);border-radius:4px;padding:1px 7px;color:var(--ac);font-weight:700;font-size:11px;display:inline-flex;align-items:center;gap:4px}
.tr-now{font-size:9px;background:rgba(0,200,255,.2);border-radius:99px;padding:0 5px;color:var(--ac)}
.tr-meta{width:100%;font-size:10px;color:var(--mu);padding-left:2px;margin-top:2px}
.mbk{position:fixed;inset:0;background:rgba(0,0,0,.77);z-index:500;display:none;align-items:center;justify-content:center;padding:10px}
.mbk.on{display:flex}
.mdl{background:var(--s1);border:1px solid var(--bd);border-radius:9px;width:100%;max-width:800px;max-height:93vh;overflow-y:auto}
.mdsm{max-width:520px}
.mhd{padding:13px 17px;border-bottom:1px solid var(--bd);display:flex;justify-content:space-between;align-items:center}
.mttl{font-weight:700;font-size:14px}
.mcls{background:none;border:none;color:var(--mu);font-size:17px;cursor:pointer;padding:0 3px}
.mbdy{padding:16px}
.mftr{padding:10px 17px;border-top:1px solid var(--bd);display:flex;gap:6px;justify-content:flex-end}
.fbar{display:flex;gap:7px;flex-wrap:wrap;align-items:flex-end;margin-bottom:9px}
.sgrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:9px;margin-bottom:12px}
.sc{background:var(--s1);border:1px solid var(--bd);border-radius:7px;padding:12px;position:relative;overflow:hidden}
.sc::after{content:'';position:absolute;top:0;left:0;right:0;height:3px}
.scy::after{background:var(--ac)}.spu::after{background:var(--ac2)}.sgn::after{background:var(--ok)}.syw::after{background:var(--wn)}.srd::after{background:var(--er)}.sorg::after{background:#f59e0b}
.snum{font-size:25px;font-weight:700;font-family:var(--mo);margin-bottom:2px}
.slbl{font-size:11px;color:var(--mu)}
.bwrap{display:flex;flex-direction:column;gap:5px}
.brow{display:flex;align-items:center;gap:7px}
.blbl{width:130px;font-size:11px;color:var(--mu);text-align:right;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.btrk{flex:1;height:15px;background:var(--bg);border-radius:3px;overflow:hidden}
.bfil{height:100%;background:var(--ac);border-radius:3px;transition:width .5s}
.bval{width:30px;font-size:11px;font-family:var(--mo)}
#standbyCard:hover{background:rgba(245,158,11,.1)!important;border-color:#f59e0b!important}
#standbyPanel input[type=number],#standbyPanel input[type=text]{border-color:rgba(245,158,11,.35)}
#standbyPanel input:focus{border-color:#f59e0b!important}
.tw{position:fixed;bottom:14px;right:14px;z-index:999;display:flex;flex-direction:column;gap:6px}
.toast{background:var(--s1);border:1px solid var(--bd);border-radius:6px;padding:9px 13px;font-size:12px;min-width:200px;animation:slin .25s ease}
.toast.success{border-color:var(--ok)}.toast.error{border-color:var(--er)}.toast.warning{border-color:var(--wn)}
@keyframes slin{from{transform:translateX(110%);opacity:0}to{transform:none;opacity:1}}
.lding{display:flex;align-items:center;justify-content:center;padding:40px;color:var(--mu);gap:8px}
.spin{width:16px;height:16px;border:2px solid var(--bd);border-top-color:var(--ac);border-radius:50%;animation:spn .8s linear infinite}
@keyframes spn{to{transform:rotate(360deg)}}
.hwarn{background:rgba(239,68,68,.08);border:1px solid var(--er);border-radius:7px;padding:10px;display:flex;align-items:center;gap:8px;margin-bottom:10px}
.sth{display:none;margin-top:4px}
input[type=checkbox]{width:14px;height:14px;cursor:pointer;accent-color:var(--ac)}
@media(max-width:768px){.nav-brand{display:none}.pg{padding:8px}.fgrid{grid-template-columns:1fr}}

/* ── ADMIN STYLES ── */
.admin-banner{background:linear-gradient(135deg,rgba(245,158,11,.15),rgba(239,68,68,.08));border:1px solid rgba(245,158,11,.4);border-radius:8px;padding:12px 16px;margin-bottom:12px;display:flex;align-items:center;gap:10px}
.admin-tabs{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:12px}
.atab{padding:7px 14px;font-size:12px;font-weight:700;cursor:pointer;border-radius:6px;border:1px solid var(--bd);background:transparent;color:var(--mu);transition:all .2s;font-family:var(--fn)}
.atab:hover{border-color:#f59e0b;color:#f59e0b}
.atab.on{background:rgba(245,158,11,.15);border-color:#f59e0b;color:#f59e0b}
.admin-sub{display:none}.admin-sub.on{display:block}
.edit-row{transition:background .15s}
.edit-row:hover td{background:rgba(245,158,11,.04)!important}
.inline-edit{background:var(--bg);border:1px solid var(--ac);border-radius:4px;padding:3px 7px;color:var(--tx);font-family:var(--fn);font-size:12px;width:100%;min-width:80px}
.inline-edit:focus{outline:none;border-color:var(--wn)}
.adm-stat-badge{display:inline-block;padding:2px 8px;border-radius:4px;font-size:11px;font-weight:700;cursor:pointer}
.row-changed td{background:rgba(245,158,11,.06)!important;border-left:3px solid var(--wn)!important}
.bulk-sel{width:16px;height:16px;accent-color:#f59e0b;cursor:pointer}
.adm-toolbar{display:flex;gap:6px;flex-wrap:wrap;align-items:center;padding:8px 12px;background:rgba(245,158,11,.05);border:1px solid rgba(245,158,11,.2);border-radius:6px;margin-bottom:10px}
.adm-count{font-size:11px;color:var(--mu);margin-left:auto}

/* ── INFOGRAPHIC SCOPED ── */
#infographic-host{font-family:"Sarabun",sans-serif!important}
 {

  --bg:#0a0f1e;
  --bg2:#0f1629;
  --card:#131d35;
  --card2:#18243e;
  --bd:rgba(255,255,255,.08);
  --ac1:#00d4ff;
  --ac2:#7c5cfc;
  --ac3:#00e5a0;
  --ac4:#ff6b35;
  --ac5:#ffd166;
  --tx:#e8edf8;
  --mu:#8892aa;
  --fn:'Sarabun',sans-serif;
  --mo:'IBM Plex Mono',monospace;
}
#infographic-host * {
box-sizing:border-box;margin:0;padding:0}
#infographic-host body {

  font-family:var(--fn);
  background:var(--bg);
  color:var(--tx);
  min-height:100vh;
  overflow-x:hidden;
}
#infographic-host /* ── HERO ─────────────────────────── */
.hero {

  position:relative;
  padding:60px 40px 50px;
  text-align:center;
  overflow:hidden;
}
#infographic-host .hero::before {

  content:'';
  position:absolute;inset:0;
  background:
    radial-gradient(ellipse 700px 400px at 50% -10%,rgba(0,212,255,.18),transparent),
    radial-gradient(ellipse 400px 300px at 20% 60%,rgba(124,92,252,.12),transparent),
    radial-gradient(ellipse 300px 200px at 80% 80%,rgba(0,229,160,.08),transparent);
  pointer-events:none;
}
#infographic-host .hero-pill {

  display:inline-flex;align-items:center;gap:8px;
  background:rgba(0,212,255,.1);border:1px solid rgba(0,212,255,.3);
  border-radius:20px;padding:5px 16px;font-size:12px;
  color:var(--ac1);font-weight:600;letter-spacing:.5px;
  margin-bottom:20px;
}
#infographic-host .hero h1 {

  font-size:clamp(28px,5vw,52px);
  font-weight:800;line-height:1.1;
  background:linear-gradient(135deg,#fff 20%,var(--ac1) 60%,var(--ac2) 100%);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  margin-bottom:8px;
}
#infographic-host .hero .sub {

  font-size:16px;color:var(--mu);font-weight:400;margin-bottom:30px;
}
#infographic-host .hero-stats {

  display:flex;justify-content:center;gap:32px;flex-wrap:wrap;
}
#infographic-host .hero-stat {

  text-align:center;
}
#infographic-host .hero-stat .n {

  font-family:var(--mo);font-size:32px;font-weight:600;
  color:var(--ac1);line-height:1;
}
#infographic-host .hero-stat .l {
font-size:12px;color:var(--mu);margin-top:4px;}
#infographic-host /* ── SECTION ──────────────────────── */
.section {
padding:0 24px 40px;}
#infographic-host .sec-title {

  display:flex;align-items:center;gap:12px;
  font-size:13px;font-weight:700;letter-spacing:1.5px;
  text-transform:uppercase;color:var(--mu);
  margin-bottom:20px;
  padding-bottom:10px;
  border-bottom:1px solid var(--bd);
}
#infographic-host .sec-title::before {

  content:'';display:block;
  width:3px;height:18px;border-radius:2px;
  background:var(--ac1);
}
#infographic-host /* ── USER ROLES ───────────────────── */
.roles {
display:grid;grid-template-columns:repeat(3,1fr);gap:12px;}
#infographic-host .role-card {

  background:var(--card);
  border:1px solid var(--bd);
  border-radius:12px;
  padding:18px;
  position:relative;
  overflow:hidden;
  transition:transform .2s,border-color .2s;
}
#infographic-host .role-card:hover {
transform:translateY(-2px);}
#infographic-host .role-card::after {

  content:'';position:absolute;
  top:0;left:0;right:0;height:3px;
  border-radius:12px 12px 0 0;
}
#infographic-host .role-card.r1::after {
background:var(--ac1);}
#infographic-host .role-card.r2::after {
background:var(--ac2);}
#infographic-host .role-card.r3::after {
background:var(--ac4);}
#infographic-host .role-card:hover.r1 {
border-color:rgba(0,212,255,.3);}
#infographic-host .role-card:hover.r2 {
border-color:rgba(124,92,252,.3);}
#infographic-host .role-card:hover.r3 {
border-color:rgba(255,107,53,.3);}
#infographic-host .role-icon {
font-size:28px;margin-bottom:10px;}
#infographic-host .role-name {
font-size:15px;font-weight:700;margin-bottom:4px;}
#infographic-host .role-desc {
font-size:12px;color:var(--mu);line-height:1.5;}
#infographic-host .role-badge {

  display:inline-block;
  margin-top:8px;padding:2px 8px;border-radius:8px;
  font-size:10px;font-weight:700;letter-spacing:.3px;
}
#infographic-host .r1 .role-badge {
background:rgba(0,212,255,.15);color:var(--ac1);}
#infographic-host .r2 .role-badge {
background:rgba(124,92,252,.15);color:var(--ac2);}
#infographic-host .r3 .role-badge {
background:rgba(255,107,53,.15);color:var(--ac4);}
#infographic-host /* ── WORKFLOW ─────────────────────── */
.workflow {
display:flex;flex-direction:column;gap:0;}
#infographic-host .wf-step {

  display:flex;gap:16px;align-items:flex-start;
  position:relative;
}
#infographic-host .wf-step:not(:last-child)::after {

  content:'';
  position:absolute;
  left:19px;top:40px;
  width:2px;
  height:calc(100% - 8px);
  background:linear-gradient(to bottom,var(--ac1),transparent);
  opacity:.3;
}
#infographic-host .wf-num {

  flex-shrink:0;
  width:38px;height:38px;
  border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  font-family:var(--mo);font-size:14px;font-weight:600;
  position:relative;z-index:1;
}
#infographic-host .wf-num.c1 {
background:rgba(0,212,255,.15);border:1px solid rgba(0,212,255,.4);color:var(--ac1);}
#infographic-host .wf-num.c2 {
background:rgba(0,229,160,.15);border:1px solid rgba(0,229,160,.4);color:var(--ac3);}
#infographic-host .wf-num.c3 {
background:rgba(124,92,252,.15);border:1px solid rgba(124,92,252,.4);color:var(--ac2);}
#infographic-host .wf-num.c4 {
background:rgba(255,107,53,.15);border:1px solid rgba(255,107,53,.4);color:var(--ac4);}
#infographic-host .wf-num.c5 {
background:rgba(255,209,102,.15);border:1px solid rgba(255,209,102,.4);color:var(--ac5);}
#infographic-host .wf-body {

  flex:1;
  background:var(--card);
  border:1px solid var(--bd);
  border-radius:10px;
  padding:14px 16px;
  margin-bottom:14px;
}
#infographic-host .wf-body .wt {
font-size:14px;font-weight:700;margin-bottom:4px;}
#infographic-host .wf-body .wd {
font-size:12px;color:var(--mu);line-height:1.6;}
#infographic-host .wf-body .who {

  display:inline-flex;align-items:center;gap:4px;
  font-size:11px;font-weight:600;margin-top:6px;
  opacity:.8;
}
#infographic-host /* ── STATUS BADGES ────────────────── */
.status-grid {

  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:10px;
}
#infographic-host .status-card {

  background:var(--card);
  border:1px solid var(--bd);
  border-radius:10px;
  padding:12px 14px;
  display:flex;align-items:center;gap:12px;
}
#infographic-host .status-dot {
width:10px;height:10px;border-radius:50%;flex-shrink:0;}
#infographic-host .status-info .sn {
font-size:13px;font-weight:700;margin-bottom:2px;}
#infographic-host .status-info .sd {
font-size:11px;color:var(--mu);}
#infographic-host .status-info .sact {

  display:inline-block;margin-top:4px;
  font-size:10px;font-weight:600;
  background:rgba(255,255,255,.06);
  border-radius:4px;padding:1px 6px;
  color:var(--mu);
}
#infographic-host /* ── NAV TABS ─────────────────────── */
.tabs-grid {

  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:10px;
}
#infographic-host .tab-card {

  background:var(--card);
  border:1px solid var(--bd);
  border-radius:10px;
  padding:14px;
  display:flex;gap:12px;align-items:flex-start;
  transition:all .2s;
  cursor:default;
}
#infographic-host .tab-card:hover {

  background:var(--card2);
  border-color:rgba(0,212,255,.2);
  transform:translateX(3px);
}
#infographic-host .tab-emoji {
font-size:24px;flex-shrink:0;line-height:1;}
#infographic-host .tab-info .tn {
font-size:13px;font-weight:700;margin-bottom:3px;}
#infographic-host .tab-info .td {
font-size:11px;color:var(--mu);line-height:1.5;}
#infographic-host /* ── TIPS ─────────────────────────── */
.tips-grid {
display:flex;flex-direction:column;gap:10px;}
#infographic-host .tip-row {

  display:flex;gap:12px;align-items:flex-start;
  background:var(--card);
  border:1px solid var(--bd);
  border-radius:10px;
  padding:12px 14px;
}
#infographic-host .tip-icon {

  font-size:16px;flex-shrink:0;
  width:32px;height:32px;
  border-radius:8px;
  display:flex;align-items:center;justify-content:center;
  background:rgba(255,255,255,.05);
}
#infographic-host .tip-txt .tl {
font-size:13px;font-weight:700;margin-bottom:2px;}
#infographic-host .tip-txt .td {
font-size:12px;color:var(--mu);line-height:1.5;}
#infographic-host /* ── HFNC ALERT ─────────────────────── */
.alert-box {

  background:rgba(255,107,53,.08);
  border:1px solid rgba(255,107,53,.3);
  border-radius:12px;
  padding:16px 20px;
  display:flex;gap:14px;align-items:flex-start;
}
#infographic-host .alert-icon {
font-size:28px;flex-shrink:0;}
#infographic-host .alert-content .al {
font-size:15px;font-weight:700;color:#ff8c5a;margin-bottom:6px;}
#infographic-host .alert-content .ad {
font-size:12px;color:var(--mu);line-height:1.6;}
#infographic-host .alert-limits {

  display:flex;gap:10px;margin-top:10px;flex-wrap:wrap;
}
#infographic-host .alert-limit {

  background:rgba(255,107,53,.12);
  border:1px solid rgba(255,107,53,.25);
  border-radius:8px;
  padding:6px 12px;text-align:center;
}
#infographic-host .alert-limit .ln {
font-family:var(--mo);font-size:20px;font-weight:600;color:#ff8c5a;}
#infographic-host .alert-limit .ll {
font-size:10px;color:var(--mu);}
#infographic-host /* ── ADMIN SECTION ─────────────────── */
.admin-grid {
display:grid;grid-template-columns:repeat(2,1fr);gap:10px;}
#infographic-host .admin-card {

  background:var(--card);
  border:1px solid rgba(255,209,102,.15);
  border-radius:10px;
  padding:12px 14px;
}
#infographic-host .admin-card .at {
font-size:13px;font-weight:700;margin-bottom:4px;color:#ffd166;}
#infographic-host .admin-card .ad {
font-size:11px;color:var(--mu);line-height:1.5;}
#infographic-host /* ── DIVIDER ───────────────────────── */
.divider {

  height:1px;
  background:linear-gradient(to right,transparent,var(--bd),transparent);
  margin:32px 0;
}
#infographic-host /* ── FOOTER ────────────────────────── */
.footer {

  text-align:center;
  padding:20px 24px 40px;
  color:var(--mu);font-size:12px;
}
#infographic-host .footer .fv {

  font-family:var(--mo);
  color:var(--ac1);
  font-size:11px;margin-top:4px;
}
#infographic-host /* ── PRINT / EXPORT ────────────────── */
@media print {

  body{background:#fff;color:#000;}
  .hero::before{display:none;}
}
#infographic-host /* ── ANIMATION ─────────────────────── */
@keyframes fadeUp {
from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:none}}
#infographic-host .hero,#infographic-host .roles,#infographic-host .workflow,#infographic-host .status-grid,#infographic-host .tabs-grid,#infographic-host .tips-grid {

  animation:fadeUp .5s ease both;
}
</style>
</head>
<body>

<div id="cfgBanner">⚠️ กรุณาตั้งค่า SUPABASE_URL และ SUPABASE_ANON_KEY ในส่วน CONFIG ด้านบนของ script ก่อนใช้งาน</div>

<nav class="nav">
  <div class="nav-brand">🏥 เครื่องมือแพทย์</div>
  <div class="nav-tabs">
    <div class="nt on" onclick="showPg('loan',this)">📋 ลงบันทึกแจ้งยืม</div>
    <div class="nt" onclick="showPg('rec',this)">📝 ลงบันทึกยืม-คืน-โอน</div>
    <div class="nt" onclick="showPg('daily',this)">📅 สรุปรายการยืมประจำวัน</div>
    <div class="nt" onclick="showPg('dash',this)">📊 Dashboard</div>
    <div class="nt" onclick="showPg('all',this)">🗂 ข้อมูลทั้งหมด</div>
    <div class="nt" onclick="showPg('sum',this)">📈 สถิติ</div>
    <div class="nt admin-tab" onclick="showPg('admin',this)">⚙️ Admin</div>
    <div class="nt" onclick="showPg('manual',this)" style="color:#00c8ff;font-weight:700">📖 คู่มือ</div>
  </div>
</nav>
<div class="tw" id="tw"></div>

<!-- LOAN PAGE -->
<div class="pg on" id="pg-loan">
  <div class="card">
    <div class="ctitle">📋 ลงบันทึกแจ้งยืมเครื่องมือแพทย์
      <span style="font-size:12px;color:var(--wn);text-transform:none;letter-spacing:0;font-weight:700;background:rgba(245,158,11,.12);border:1px solid rgba(245,158,11,.35);border-radius:5px;padding:2px 9px;margin-left:6px">( สำหรับแผนกต่างๆ กรอกข้อมูลขอยืม )</span>
    </div>
    <div class="fgrid">
      <div class="fg"><label>วันที่ยืม *</label><input type="date" id="loanDate"></div>
      <div class="fg"><label>ชื่อผู้ยืม *</label><input type="text" id="borrowerName" placeholder="ชื่อ-นามสกุล"></div>
      <div class="fg"><label>แผนกที่ยืม *</label>
        <div class="ssw"><input type="text" id="deptSearch" placeholder="พิมพ์ชื่อแผนก..." oninput="filterSSL('deptList',this.value)" onfocus="openSSL('deptList')" autocomplete="off">
          <div class="ssl" id="deptList"></div></div>
        <div class="seld" id="selDeptDisp"></div>
        <input type="hidden" id="selDeptCode"><input type="hidden" id="selDeptName">
        <input type="text" id="deptOther" placeholder="ระบุชื่อแผนก/หน่วยงาน..." style="display:none;margin-top:6px;border-color:var(--wn)" oninput="updateDeptOther()">
      </div>
      <div class="fg"><label>ชื่อ-นามสกุลผู้ป่วย *</label><input type="text" id="patientName" placeholder="ชื่อ-นามสกุลผู้ป่วย"></div>
    </div>
  </div>
  <div class="card">
    <div class="ctitle">🔧 เลือกรายการเครื่องมือแพทย์ <span style="font-size:11px;color:var(--mu);font-weight:400;text-transform:none;letter-spacing:0">(เลือกได้ 1 รายการ — คลิกที่การ์ด)</span></div>

    

    <!-- DIVIDER -->
    <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
      <div style="flex:1;height:1px;background:var(--bd)"></div>
      <span style="font-size:11px;color:var(--mu)">หรือเลือกเครื่องมืออื่น</span>
      <div style="flex:1;height:1px;background:var(--bd)"></div>
    </div>

    <div id="eqCardGrid" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:8px;margin-bottom:12px"></div>
    <div class="bgrp">
      <button class="btn bp" onclick="submitLoan()">📤 ส่งแบบฟอร์มยืม</button>
      <button class="btn bg" onclick="clearLoan()">🗑 ล้างข้อมูล</button>
    </div>
  </div>
</div>

<!-- RECORD PAGE -->
<div class="pg" id="pg-rec">
  <div class="card">
    <div class="ctitle">🔐 ลงบันทึกยืม-คืน-โอน
      <span style="font-size:12px;color:var(--ac);text-transform:none;letter-spacing:0;font-weight:700;background:rgba(0,200,255,.08);border:1px solid rgba(0,200,255,.25);border-radius:5px;padding:2px 9px;margin-left:6px">( สำหรับ จนท.แผนกบริการเครื่องมือแพทย์ฯ )</span>
    </div>
    <div class="fbar">
      <div class="fg"><label>ค้นหา</label><input type="text" id="recSearch" placeholder="ชื่อ, แผนก, หมายเลข..." oninput="renderRec()" style="min-width:130px"></div>
      <div class="fg"><label>ชนิดเครื่องมือ</label><select id="recEq" onchange="renderRec()" style="min-width:140px"><option value="">ทุกรายการ</option></select></div>
      <div class="fg"><label>แผนก</label><select id="recDept" onchange="renderRec()" style="min-width:110px"><option value="">ทั้งแผนก</option></select></div>
      <div class="fg"><label>สถานะ</label>
        <select id="recStat" onchange="renderRec()">
          <option value="">ทั้งหมด</option>
          <option value="PENDING">รอดำเนินการจ่าย</option>
          <option value="ACTIVE">ยืม</option>
          <option value="TRANSFERRED">โอน/มีประวัติการโอน</option>
          <option value="RETURNED">คืน</option>
          <option value="ACC_PENDING">คืนแต่มีอุปกรณ์ค้าง</option>
          <option value="CANCELLED">ยกเลิก</option>
        </select></div>
      <div class="fg"><label>จาก</label><input type="date" id="recDateS" onchange="renderRec()"></div>
      <div class="fg"><label>ถึง</label><input type="date" id="recDateE" onchange="renderRec()"></div>
      <button class="btn bg sm" onclick="loadAll()">🔄 โหลด</button>
      <div class="fg"><label style="font-size:9px">ดาวน์โหลด</label>
        <div style="display:flex;gap:4px">
          <button class="btn bg sm" onclick="dlPage('rec','csv')">CSV</button>
          <button class="btn bg sm" onclick="dlPage('rec','xlsx')">XLS</button>
          <button class="btn bg sm" onclick="dlPage('rec','xlsx')">XLSX</button>
          <button class="btn bg sm" onclick="dlPage('rec','pdf')">PDF</button>
        </div>
      </div>
    </div>
    <div class="dtw"><table class="dt" id="recTbl">
      <thead><tr>
        <th style="width:36px">#</th>
        <th>ข้อมูลใบยืม / รายการเครื่องมือ</th>
        <th style="width:120px">หมายเลขเครื่อง</th>
        <th style="width:150px">สถานะ</th>
        <th>การดำเนินการ</th>
      </tr></thead>
      <tbody id="recBody"><tr><td colspan="5"><div class="lding"><div class="spin"></div> กำลังโหลด...</div></td></tr></tbody>
    </table></div>
  </div>
</div>

<!-- DAILY PAGE -->
<div class="pg" id="pg-daily">
  <div class="card">
    <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">
      <div class="ctitle" style="margin:0">📅 สรุปรายการยืมเครื่องมือประจำวัน</div>
      <div style="display:flex;gap:4px;align-self:flex-end">
        <button class="btn bg sm" onclick="dlPage('daily','csv')">CSV</button>
        <button class="btn bg sm" onclick="dlPage('daily','xlsx')">XLS</button>
        <button class="btn bg sm" onclick="dlPage('daily','xlsx')">XLSX</button>
        <button class="btn bg sm" onclick="dlPage('daily','pdf')">PDF</button>
        <button class="btn bp sm" onclick="loadAll()">🔄 Refresh</button>
        <span id="dailyTs" style="font-size:11px;color:var(--mu);align-self:center;margin-left:4px"></span>
      </div>
    </div>
    <div class="fbar" style="margin-bottom:6px">
      <div class="fg"><label>ค้นหา</label><input type="text" id="dailySearch" placeholder="หมายเลขเครื่อง, ผู้ป่วย, ผู้ให้ยืม..." oninput="renderDaily()" style="min-width:220px"></div>
      <div class="fg"><label>ชนิดเครื่องมือ</label><select id="dailyFilter" onchange="renderDaily()" style="min-width:160px"><option value="">— ทุกชนิด —</option></select></div>
      <div class="fg"><label>แผนก</label><select id="dailyDept" onchange="renderDaily()" style="min-width:120px"><option value="">ทั้งแผนก</option></select></div>
      <div class="fg"><label>วันที่ยืม จาก</label><input type="date" id="dailyDateS" onchange="renderDaily()"></div>
      <div class="fg"><label>วันที่ยืม ถึง</label><input type="date" id="dailyDateE" onchange="renderDaily()"></div>
      <button class="btn bg sm" onclick="clearDailyFilter()" style="align-self:flex-end">✕ ล้าง</button>
    </div>
  </div>
  <div id="dailyCont"><div class="lding"><div class="spin"></div> กำลังโหลด...</div></div>
</div>

<!-- DASHBOARD PAGE -->
<div class="pg" id="pg-dash">
  <div class="card" style="padding:8px 13px;margin-bottom:10px">
    <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px">
      <div class="ctitle" style="margin:0">📊 Dashboard</div>
      <div style="display:flex;gap:4px;align-items:center;flex-wrap:wrap">
        <button class="btn bg sm" onclick="dlPage('dash','csv')">CSV</button>
        <button class="btn bg sm" onclick="dlPage('dash','xlsx')">XLS</button>
        <button class="btn bg sm" onclick="dlPage('dash','pdf')">PDF</button>
        <button class="btn bp sm" onclick="updateDash()">🔄</button>
      </div>
    </div>
  </div>
  <div id="dashCont"><div class="lding"><div class="spin"></div></div></div>
</div>

<!-- ALL DATA PAGE -->
<div class="pg" id="pg-all">
  <div class="card">
    <div class="ctitle">🗂 ข้อมูลทั้งหมด</div>
    <div class="fbar">
      <div class="fg"><label>ค้นหา</label><input type="text" id="allSearch" oninput="renderAll()" style="min-width:120px" placeholder="ค้นหา..."></div>
      <div class="fg"><label>แผนก</label><select id="allDept" onchange="renderAll()"><option value="">ทั้งแผนก</option></select></div>
      <div class="fg"><label>เครื่องมือ</label><select id="allEq" onchange="renderAll()"><option value="">ทุกรายการ</option></select></div>
      <div class="fg"><label>สถานะ</label>
        <select id="allStat" onchange="renderAll()">
          <option value="">ทั้งหมด</option>
          <option value="PENDING">รอดำเนินการจ่าย</option>
          <option value="ACTIVE">ยืม</option>
          <option value="RETURNED">คืน</option>
          <option value="ACC_PENDING">ค้างอุปกรณ์</option>
          <option value="CANCELLED">ยกเลิก</option>
        </select></div>
      <div class="fg"><label>จาก</label><input type="date" id="allDateS" onchange="renderAll()"></div>
      <div class="fg"><label>ถึง</label><input type="date" id="allDateE" onchange="renderAll()"></div>
      <div class="fg"><label style="font-size:9px">ดาวน์โหลด</label>
        <div style="display:flex;gap:4px">
          <button class="btn bg sm" onclick="dlPage('all','csv')">CSV</button>
          <button class="btn bg sm" onclick="dlPage('all','xlsx')">XLS</button>
          <button class="btn bg sm" onclick="dlPage('all','xlsx')">XLSX</button>
          <button class="btn bg sm" onclick="dlPage('all','pdf')">PDF</button>
        </div>
      </div>
    </div>
    <div class="dtw"><table class="dt">
      <thead><tr><th>LoanID / #Item</th><th>วันที่ยืม</th><th>ผู้ยืม</th><th>แผนก</th><th>ผู้ป่วย</th><th>เครื่องมือ</th><th>หมายเลขเครื่อง</th><th>อุปกรณ์</th><th>วันที่ใช้</th><th>สถานะ</th><th>รายละเอียด</th></tr></thead>
      <tbody id="allBody"><tr><td colspan="11"><div class="lding"><div class="spin"></div></div></td></tr></tbody>
    </table></div>
  </div>
</div>

<!-- SUMMARY PAGE -->
<div class="pg" id="pg-sum">

  <!-- ══ GLOBAL FILTER ══ -->
  <div class="card" style="border-color:rgba(0,200,255,.3);background:linear-gradient(135deg,rgba(0,200,255,.04),rgba(124,58,237,.03))">
    <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">
      <div class="ctitle" style="margin:0">
        🔵 Global Filter
        <span style="font-size:10px;color:var(--mu);font-weight:400;text-transform:none;letter-spacing:0;margin-left:6px">กรองทุก section พร้อมกัน</span>
      </div>
      <div style="display:flex;gap:4px">
        <button class="btn bg sm" onclick="dlPage('sum','csv')">CSV</button>
        <button class="btn bg sm" onclick="dlPage('sum','xlsx')">XLS</button>
        <button class="btn bg sm" onclick="dlPage('sum','pdf')">PDF</button>
      </div>
    </div>
    <div class="fbar">
      <div class="fg"><label>จากวันที่</label><input type="date" id="sumS" onchange="loadSum()"></div>
      <div class="fg"><label>ถึงวันที่</label><input type="date" id="sumE" onchange="loadSum()"></div>
      <div class="fg"><label>แผนก</label><select id="sumDept" onchange="loadSum()" style="min-width:110px"><option value="">ทั้งแผนก</option></select></div>
      <div class="fg"><label>เครื่องมือ</label><select id="sumEq" onchange="loadSum()" style="min-width:150px"><option value="">ทุกรายการ</option></select></div>
      <div class="fg"><label>สถานะ</label>
        <select id="sumStat" onchange="loadSum()">
          <option value="">ทั้งหมด</option>
          <option value="ACTIVE">ยืม</option>
          <option value="RETURNED">คืน</option>
          <option value="ACC_PENDING">อุปกรณ์ค้าง</option>
        </select>
      </div>
      <button class="btn bp sm" onclick="loadSum()">🔍 ดู</button>
      <button class="btn bg sm" onclick="clearSumFilter()">✕ ล้างทั้งหมด</button>
    </div>
    <div id="sumFilterBadges" style="display:none;margin-top:8px;gap:6px;flex-wrap:wrap"></div>
  </div>

  <!-- ══ SECTION OVERRIDE FILTERS ══ -->
  <div class="card" style="padding:12px 16px">
    <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
      <span style="font-size:11px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.5px">🎛 Filter เพิ่มเติมแต่ละ Section</span>
      <span style="font-size:10px;color:var(--mu)">(override Global Filter เฉพาะ section นั้น)</span>
    </div>
    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:10px">

      <!-- กราฟแท่ง -->
      <div style="background:var(--s2);border-radius:7px;padding:10px 12px;border-left:3px solid var(--ac)">
        <div style="font-size:11px;font-weight:700;color:var(--ac);margin-bottom:7px">📊 กราฟแท่ง</div>
        <div style="display:flex;gap:6px;align-items:center;flex-wrap:wrap">
          <select id="secBarEq" onchange="renderSum({})" style="flex:1;min-width:120px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <button class="btn bg sm" style="font-size:10px;padding:3px 7px" onclick="document.getElementById('secBarEq').value='';renderSum({})">✕</button>
        </div>
      </div>

      <!-- ตารางรายวัน -->
      <div style="background:var(--s2);border-radius:7px;padding:10px 12px;border-left:3px solid var(--ac2)">
        <div style="font-size:11px;font-weight:700;color:var(--ac2);margin-bottom:7px">📅 ตารางยืม-คืนรายวัน</div>
        <div style="display:flex;gap:6px;align-items:center;flex-wrap:wrap">
          <select id="secDayEq" onchange="renderSum({})" style="flex:1;min-width:120px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <button class="btn bg sm" style="font-size:10px;padding:3px 7px" onclick="document.getElementById('secDayEq').value='';renderSum({})">✕</button>
        </div>
      </div>

      <!-- ค่าเฉลี่ย -->
      <div style="background:var(--s2);border-radius:7px;padding:10px 12px;border-left:3px solid var(--ok)">
        <div style="font-size:11px;font-weight:700;color:var(--ok);margin-bottom:7px">📈 ค่าเฉลี่ยการใช้งาน (Snapshot)</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:5px;margin-bottom:5px">
          <input type="date" id="secAvgS" onchange="renderSum({})" placeholder="จากวันที่" style="font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
          <input type="date" id="secAvgE" onchange="renderSum({})" placeholder="ถึงวันที่" style="font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
        </div>
        <div style="display:flex;gap:5px;align-items:center">
          <select id="secAvgEq" onchange="renderSum({})" style="flex:1;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ทุกเครื่องมือ —</option>
          </select>
          <button class="btn bg sm" style="font-size:10px;padding:3px 7px" onclick="['secAvgS','secAvgE','secAvgEq'].forEach(function(id){document.getElementById(id).value='';});renderSum({})">✕</button>
        </div>
        <div style="margin-top:5px;font-size:10px;color:var(--mu)">ข้อมูลจาก daily_snapshots (Cron 23:59 น.)</div>
      </div>

      <!-- ยืมเกิน 1 เดือน -->
      <div style="background:var(--s2);border-radius:7px;padding:10px 12px;border-left:3px solid var(--er)">
        <div style="font-size:11px;font-weight:700;color:var(--er);margin-bottom:7px">⏰ ยืมเกิน 1 เดือน</div>
        <div style="display:flex;gap:6px;align-items:center;flex-wrap:wrap">
          <select id="secOvDept" onchange="renderSum({})" style="flex:1;min-width:120px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <select id="secOvEq" onchange="renderSum({})" style="flex:1;min-width:120px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <button class="btn bg sm" style="font-size:10px;padding:3px 7px" onclick="['secOvDept','secOvEq'].forEach(function(id){document.getElementById(id).value='';});renderSum({})">✕</button>
        </div>
      </div>

      <!-- ข้อมูลทั้งหมด -->
      <div style="background:var(--s2);border-radius:7px;padding:10px 12px;border-left:3px solid var(--wn)">
        <div style="font-size:11px;font-weight:700;color:var(--wn);margin-bottom:7px">📋 ข้อมูลการยืมทั้งหมด</div>
        <div style="display:flex;gap:6px;align-items:center;flex-wrap:wrap">
          <select id="secAllDept" onchange="renderSum({})" style="flex:1;min-width:100px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <select id="secAllEq" onchange="renderSum({})" style="flex:1;min-width:100px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
          </select>
          <select id="secAllStat" onchange="renderSum({})" style="flex:1;min-width:100px;font-size:12px;padding:4px 7px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">
            <option value="">— ตาม Global —</option>
            <option value="ACTIVE">ยืม</option>
            <option value="RETURNED">คืน</option>
            <option value="ACC_PENDING">อุปกรณ์ค้าง</option>
          </select>
          <button class="btn bg sm" style="font-size:10px;padding:3px 7px" onclick="['secAllDept','secAllEq','secAllStat'].forEach(function(id){document.getElementById(id).value='';});renderSum({})">✕</button>
        </div>
      </div>

    </div>
  </div>

  <div id="sumCont"></div>
</div>

<!-- ══════════════════════════════════════════════════════════
     ADMIN PAGE
══════════════════════════════════════════════════════════ -->
<div class="pg" id="pg-admin">
  <!-- Lock screen -->
  <div id="adminLock" style="display:flex;align-items:center;justify-content:center;min-height:60vh">
    <div style="background:var(--s1);border:2px solid #f59e0b;border-radius:14px;padding:40px 48px;text-align:center;max-width:400px;width:100%">
      <div style="font-size:52px;margin-bottom:12px">⚙️</div>
      <div style="font-size:22px;font-weight:700;color:#f59e0b;margin-bottom:6px">Admin Panel</div>
      <div style="font-size:13px;color:var(--mu);margin-bottom:22px">เฉพาะผู้ดูแลระบบเท่านั้น</div>
      <input type="password" id="adminPinInput" placeholder="รหัส Admin" maxlength="10"
        style="width:100%;padding:12px;font-size:18px;text-align:center;letter-spacing:6px;background:var(--bg);border:1px solid var(--bd);border-radius:7px;color:var(--tx);margin-bottom:12px"
        onkeydown="if(event.key==='Enter')checkAdminPin()">
      <div id="adminPinErr" style="color:var(--er);font-size:13px;margin-bottom:10px;min-height:18px"></div>
      <button class="btn badm" style="width:100%;padding:12px;font-size:15px" onclick="checkAdminPin()">🔓 เข้าสู่ระบบ Admin</button>
    </div>
  </div>

  <!-- Admin Content (hidden until pin unlocked) -->
  <div id="adminContent" style="display:none">
    <div class="admin-banner">
      <span style="font-size:24px">⚙️</span>
      <div>
        <div style="font-weight:700;color:#f59e0b;font-size:15px">Admin Panel — จัดการข้อมูลทั้งหมด</div>
        <div style="font-size:12px;color:var(--mu)">แก้ไข เพิ่ม ลบ ข้อมูลได้ทุกรายการ — การเปลี่ยนแปลงบันทึกลง Supabase ทันที</div>
      </div>
      <div style="margin-left:auto;display:flex;gap:6px">
        <button class="btn bp sm" id="admRefreshBtn" onclick="adminRefresh()">🔄 Refresh</button>
        <button class="btn bg sm" onclick="lockAdmin()">🔒 ออก</button>
      </div>
    </div>

    <!-- Sub-tabs -->
    <div class="admin-tabs">
      <button class="atab on" onclick="showAdminTab('loan-mgr',this)">📋 ใบยืม (loan_requests)</button>
      <button class="atab" onclick="showAdminTab('borrow-mgr',this)">📝 บันทึกยืม (borrow_records)</button>
      <button class="atab" onclick="showAdminTab('quick-edit',this)">⚡ แก้ไขด่วน</button>
      <button class="atab" onclick="showAdminTab('bulk-ops',this)">🗂 Bulk Operations</button>
      <button class="atab" onclick="showAdminTab('lists-mgr',this)">🗃 จัดการรายการ</button>
      <button class="atab" onclick="showAdminTab('pin-mgr',this)">🔑 จัดการรหัสผ่าน</button>
    </div>

    <!-- ── TAB 1: LOAN REQUESTS ── -->
    <div class="admin-sub on" id="adm-loan-mgr">
      <div class="card">
        <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">
          <div class="ctitle" style="margin:0;color:#f59e0b">📋 จัดการใบยืม (loan_requests)</div>
          <div style="display:flex;gap:6px;flex-wrap:wrap">
            <button class="btn badm sm" onclick="openAddLoanModal()">➕ เพิ่มใบยืมใหม่</button>
            <button class="btn bg sm" onclick="dlPage('all','csv')">📥 Export CSV</button>
          </div>
        </div>
        <div class="fbar" style="margin-bottom:8px">
          <div class="fg"><label>ค้นหา</label><input type="text" id="admLoanSearch" oninput="renderAdminLoans()" placeholder="LoanID, ชื่อ, แผนก..." style="min-width:180px"></div>
          <div class="fg"><label>สถานะ</label>
            <select id="admLoanStat" onchange="renderAdminLoans()">
              <option value="">ทั้งหมด</option>
              <option value="PENDING">PENDING</option>
              <option value="ACTIVE">ACTIVE</option>
              <option value="RETURNED">RETURNED</option>
              <option value="CANCELLED">CANCELLED</option>
            </select>
          </div>
          <div class="fg"><label>จาก</label><input type="date" id="admLoanDateS" onchange="renderAdminLoans()"></div>
          <div class="fg"><label>ถึง</label><input type="date" id="admLoanDateE" onchange="renderAdminLoans()"></div>
          <button class="btn bg sm" onclick="admClearFilter('loan')">✕ ล้าง</button>
        </div>
        <div id="admLoanCount" style="font-size:11px;color:var(--mu);margin-bottom:6px"></div>
        <div class="dtw">
          <table class="dt">
            <thead><tr>
              <th><input type="checkbox" class="bulk-sel" id="chkAllLoans" onchange="toggleSelectAll('loan',this.checked)"></th>
              <th>LoanID</th><th>วันที่ยืม</th><th>ผู้ยืม</th><th>แผนก</th><th>ผู้ป่วย</th><th>เครื่องมือ</th><th>สถานะ</th><th style="min-width:160px">Actions</th>
            </tr></thead>
            <tbody id="admLoanBody"></tbody>
          </table>
        </div>
        <div id="admLoanBulkBar" style="display:none" class="adm-toolbar" style="margin-top:8px">
          <span style="font-size:12px;font-weight:700;color:#f59e0b">เลือก <span id="loanSelCount">0</span> รายการ:</span>
          <button class="btn bwn sm" onclick="bulkChangeLoanStatus('PENDING')">→ PENDING</button>
          <button class="btn bok sm" onclick="bulkChangeLoanStatus('ACTIVE')">→ ACTIVE</button>
          <button class="btn bg sm" onclick="bulkChangeLoanStatus('RETURNED')">→ RETURNED</button>
          <button class="btn ber sm" onclick="bulkChangeLoanStatus('CANCELLED')">→ CANCELLED</button>
          <button class="btn ber sm" onclick="bulkDeleteLoans()">🗑 ลบที่เลือก</button>
        </div>
      </div>
    </div>

    <!-- ── TAB 2: BORROW RECORDS ── -->
    <div class="admin-sub" id="adm-borrow-mgr">
      <div class="card">
        <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">
          <div class="ctitle" style="margin:0;color:#f59e0b">📝 จัดการบันทึกการยืม (borrow_records)</div>
          <div style="display:flex;gap:6px">
            <button class="btn bg sm" onclick="dlAdmBorrows()">📥 Export CSV</button>
          </div>
        </div>
        <div class="fbar" style="margin-bottom:8px">
          <div class="fg"><label>ค้นหา</label><input type="text" id="admBorSearch" oninput="renderAdminBorrows()" placeholder="RecordID, LoanID, หมายเลขเครื่อง..." style="min-width:200px"></div>
          <div class="fg"><label>สถานะ</label>
            <select id="admBorStat" onchange="renderAdminBorrows()">
              <option value="">ทั้งหมด</option>
              <option value="ACTIVE">ACTIVE</option>
              <option value="RETURNED">RETURNED</option>
              <option value="ACC_PENDING">ACC_PENDING</option>
            </select>
          </div>
          <div class="fg"><label>เครื่องมือ</label>
            <select id="admBorEq" onchange="renderAdminBorrows()">
              <option value="">ทุกรายการ</option>
            </select>
          </div>
          <button class="btn bg sm" onclick="admClearFilter('borrow')">✕ ล้าง</button>
        </div>
        <div id="admBorCount" style="font-size:11px;color:var(--mu);margin-bottom:6px"></div>
        <div class="dtw">
          <table class="dt">
            <thead><tr>
              <th><input type="checkbox" class="bulk-sel" id="chkAllBors" onchange="toggleSelectAll('borrow',this.checked)"></th>
              <th>RecordID</th><th>LoanID</th><th>ผู้ให้ยืม</th><th>หมายเลขเครื่อง</th><th>เครื่องมือ</th><th>วันที่คืน</th><th>ผู้รับคืน</th><th>สถานะ</th><th style="min-width:170px">Actions</th>
            </tr></thead>
            <tbody id="admBorBody"></tbody>
          </table>
        </div>
        <div id="admBorBulkBar" style="display:none" class="adm-toolbar" style="margin-top:8px">
          <span style="font-size:12px;font-weight:700;color:#f59e0b">เลือก <span id="borSelCount">0</span> รายการ:</span>
          <button class="btn bok sm" onclick="bulkChangeBorStatus('ACTIVE')">→ ACTIVE</button>
          <button class="btn bg sm" onclick="bulkChangeBorStatus('RETURNED')">→ RETURNED</button>
          <button class="btn ber sm" onclick="bulkDeleteBorrows()">🗑 ลบที่เลือก</button>
        </div>
      </div>
    </div>

    <!-- ── TAB 3: QUICK EDIT ── -->
    <div class="admin-sub" id="adm-quick-edit">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px">
        <!-- Quick find & edit loan -->
        <div class="card">
          <div class="ctitle" style="color:#f59e0b">⚡ ค้นหาและแก้ไขใบยืม</div>
          <div class="fg" style="margin-bottom:8px"><label>ค้นหา LoanID หรือชื่อ</label>
            <input type="text" id="qeLoanSearch" placeholder="พิมพ์ LoanID หรือชื่อ..." oninput="qeSearchLoan()">
          </div>
          <div id="qeLoanResult"></div>
        </div>
        <!-- Quick find & edit borrow record -->
        <div class="card">
          <div class="ctitle" style="color:#f59e0b">⚡ ค้นหาและแก้ไขบันทึกการยืม</div>
          <div class="fg" style="margin-bottom:8px"><label>ค้นหา RecordID หรือหมายเลขเครื่อง</label>
            <input type="text" id="qeBorSearch" placeholder="พิมพ์ RecordID หรือหมายเลขเครื่อง..." oninput="qeSearchBorrow()">
          </div>
          <div id="qeBorResult"></div>
        </div>
      </div>
      <!-- Status changer -->
      <div class="card">
        <div class="ctitle" style="color:#f59e0b">🔄 เปลี่ยนสถานะด่วน</div>
        <div class="fgrid" style="margin-bottom:10px">
          <div class="fg"><label>LoanID</label><input type="text" id="qsLoanID" placeholder="LN-..."></div>
          <div class="fg"><label>สถานะใหม่</label>
            <select id="qsNewStat">
              <option value="PENDING">PENDING — รอดำเนินการ</option>
              <option value="ACTIVE">ACTIVE — ยืม</option>
              <option value="RETURNED">RETURNED — คืน</option>
              <option value="CANCELLED">CANCELLED — ยกเลิก</option>
            </select>
          </div>
        </div>
        <button class="btn badm" onclick="quickChangeStatus()">✅ เปลี่ยนสถานะ</button>
      </div>
      <!-- Date corrector -->
      <div class="card">
        <div class="ctitle" style="color:#f59e0b">📅 แก้ไขวันที่</div>
        <div class="fgrid" style="margin-bottom:10px">
          <div class="fg"><label>LoanID</label><input type="text" id="qdLoanID" placeholder="LN-..."></div>
          <div class="fg"><label>วันที่ยืมใหม่</label><input type="date" id="qdNewDate"></div>
        </div>
        <button class="btn badm" onclick="quickChangeDate()">📅 บันทึกวันที่</button>
      </div>
      <!-- Dept corrector -->
      <div class="card">
        <div class="ctitle" style="color:#f59e0b">🏥 แก้ไขแผนก/ผู้ยืม/ผู้ป่วย</div>
        <div class="fgrid" style="margin-bottom:10px">
          <div class="fg"><label>LoanID</label><input type="text" id="qfLoanID" placeholder="LN-..."></div>
          <div class="fg"><label>แผนกใหม่</label><input type="text" id="qfNewDept" placeholder="ชื่อแผนก..."></div>
          <div class="fg"><label>ผู้ยืมใหม่</label><input type="text" id="qfNewBorrower" placeholder="ชื่อ-นามสกุล..."></div>
          <div class="fg"><label>ผู้ป่วยใหม่</label><input type="text" id="qfNewPatient" placeholder="ชื่อ-นามสกุลผู้ป่วย..."></div>
        </div>
        <button class="btn badm" onclick="quickChangeFields()">💾 บันทึกข้อมูล</button>
      </div>
      <!-- Equipment No corrector -->
      <div class="card">
        <div class="ctitle" style="color:#f59e0b">🔢 แก้ไขหมายเลขเครื่อง</div>
        <div class="fgrid" style="margin-bottom:10px">
          <div class="fg"><label>RecordID (BR-...)</label><input type="text" id="qeRecID" placeholder="BR-..."></div>
          <div class="fg"><label>หมายเลขเครื่องใหม่</label><input type="text" id="qeNewEqNo" placeholder="หมายเลขเครื่อง..."></div>
          <div class="fg"><label>ผู้ให้ยืม (ถ้าต้องการแก้)</label><input type="text" id="qeNewGiver" placeholder="ชื่อผู้ให้ยืม..."></div>
        </div>
        <button class="btn badm" onclick="quickChangeEqNo()">💾 บันทึกหมายเลข</button>
      </div>
      <!-- Return date corrector -->
      <div class="card">
        <div class="ctitle" style="color:#f59e0b">↩ แก้ไขวันที่คืน / ผู้รับคืน</div>
        <div class="fgrid" style="margin-bottom:10px">
          <div class="fg"><label>RecordID (BR-...)</label><input type="text" id="qrRecID" placeholder="BR-..."></div>
          <div class="fg"><label>วันที่คืนใหม่</label><input type="date" id="qrNewRetDate"></div>
          <div class="fg"><label>ผู้รับคืน</label><input type="text" id="qrNewReceiver" placeholder="ชื่อผู้รับคืน..."></div>
          <div class="fg"><label>เหตุผลคืน</label><input type="text" id="qrNewRemark" placeholder="เหตุผล..."></div>
        </div>
        <button class="btn badm" onclick="quickChangeReturn()">💾 บันทึกการคืน</button>
      </div>
    </div>

    <!-- ── TAB 4: BULK OPS ── -->
    <div class="admin-sub" id="adm-bulk-ops">
      <div class="card" style="border-color:rgba(239,68,68,.3)">
        <div class="ctitle" style="color:var(--er)">⚠️ Bulk Operations — ใช้ด้วยความระมัดระวัง</div>
        <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:12px">

          <!-- Delete by status -->
          <div style="background:var(--s2);border-radius:7px;padding:14px">
            <div style="font-weight:700;color:var(--er);margin-bottom:8px;font-size:13px">🗑 ลบตามสถานะ</div>
            <div class="fg" style="margin-bottom:8px"><label>สถานะที่ต้องการลบ</label>
              <select id="bulkDelStat">
                <option value="CANCELLED">CANCELLED — ยกเลิก</option>
                <option value="RETURNED">RETURNED — คืนแล้ว</option>
              </select>
            </div>
            <div class="fg" style="margin-bottom:8px"><label>วันที่ยืม ก่อน (เก่ากว่า)</label><input type="date" id="bulkDelBefore"></div>
            <button class="btn ber sm" onclick="bulkDeleteByStatus()">🗑 ลบรายการที่ตรงเงื่อนไข</button>
          </div>

          <!-- Fix orphan records -->
          <div style="background:var(--s2);border-radius:7px;padding:14px">
            <div style="font-weight:700;color:var(--wn);margin-bottom:8px;font-size:13px">🔧 ซ่อมแซมข้อมูล</div>
            <div style="font-size:12px;color:var(--mu);margin-bottom:10px">ตรวจสอบ borrow_records ที่ไม่มี loan_requests รองรับ</div>
            <button class="btn bwn sm" onclick="findOrphanRecords()">🔍 ค้นหา Orphan Records</button>
            <div id="orphanResult" style="margin-top:8px;font-size:12px"></div>
          </div>

          <!-- Recalculate loan statuses -->
          <div style="background:var(--s2);border-radius:7px;padding:14px">
            <div style="font-weight:700;color:var(--ac);margin-bottom:8px;font-size:13px">🔄 คำนวณสถานะใหม่</div>
            <div style="font-size:12px;color:var(--mu);margin-bottom:10px">อัปเดตสถานะ loan_requests ตาม borrow_records ทั้งหมด</div>
            <button class="btn bp sm" onclick="recalcAllStatuses()">⚙️ Recalculate All</button>
            <div id="recalcResult" style="margin-top:8px;font-size:12px"></div>
          </div>

          <!-- Export all data -->
          <div style="background:var(--s2);border-radius:7px;padding:14px">
            <div style="font-weight:700;color:var(--ok);margin-bottom:8px;font-size:13px">📤 Export ข้อมูลทั้งหมด</div>
            <div style="font-size:12px;color:var(--mu);margin-bottom:10px">ดาวน์โหลดข้อมูลทั้งหมดในรูปแบบ JSON/CSV</div>
            <div style="display:flex;gap:6px;flex-wrap:wrap">
              <button class="btn bok sm" onclick="exportAllJSON()">📄 JSON</button>
              <button class="btn bg sm" onclick="dlPage('all','csv')">📊 CSV</button>
              <button class="btn bg sm" onclick="dlPage('all','xlsx')">📗 XLS</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Preview area for bulk ops -->
      <div class="card" id="bulkPreviewArea" style="display:none">
        <div class="ctitle" style="color:var(--wn)">👁 Preview รายการที่จะดำเนินการ</div>
        <div id="bulkPreviewContent"></div>
        <div class="bgrp">
          <button class="btn ber" id="bulkConfirmBtn" onclick="executeBulkDelete()">⚠️ ยืนยันการลบ</button>
          <button class="btn bg" onclick="cancelBulkPreview()">ยกเลิก</button>
        </div>
      </div>
    </div>
    <!-- ── TAB 5: LISTS MANAGER ── -->
    <div class="admin-sub" id="adm-lists-mgr">

      <!-- sub-sub tabs -->
      <div style="display:flex;gap:4px;flex-wrap:wrap;margin-bottom:12px">
        <button class="atab on" id="ltab-dept" onclick="showListTab('dept',this)" style="font-size:11px">🏥 แผนกที่ยืม</button>
        <button class="atab" id="ltab-staff" onclick="showListTab('staff',this)" style="font-size:11px">👤 ผู้ให้ยืม / ผู้รับคืน</button>
        <button class="atab" id="ltab-eq" onclick="showListTab('eq',this)" style="font-size:11px">🔧 รายการเครื่องมือ</button>
        <button class="atab" id="ltab-acc" onclick="showListTab('acc',this)" style="font-size:11px">📦 อุปกรณ์ประกอบ</button>
        <button class="atab" id="ltab-eqnum" onclick="showListTab('eqnum',this)" style="font-size:11px">🔢 หมายเลขเครื่องมือ</button>
        <button class="atab" id="ltab-retrem" onclick="showListTab('retrem',this)" style="font-size:11px">↩ เหตุผลการรับคืน</button>
        <button class="atab" id="ltab-hfncset" onclick="showListTab('hfncset',this)" style="font-size:11px">💨 HFNC Set</button>
      </div>

      <!-- DEPT LIST -->
      <div id="lsec-dept">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">🏥 จัดการรายชื่อแผนก (Dropdown)</div>
            <div style="font-size:11px;color:var(--mu)">การเปลี่ยนแปลงมีผลทันทีกับ Dropdown ทุกแห่งในระบบ</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <!-- Left: current list -->
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายชื่อแผนกปัจจุบัน <span id="deptCountBadge" style="background:rgba(0,200,255,.15);color:var(--ac);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <input type="text" id="deptFilterInput" placeholder="🔍 กรองรายชื่อ..." oninput="renderDeptList()" style="margin-bottom:8px;font-size:12px">
              <div id="deptListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveDeptItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveDeptItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedDept()">🗑 ลบที่เลือก</button>
              </div>
            </div>
            <!-- Right: add/edit -->
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่ม / แก้ไขแผนก</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:8px"><label>รหัสแผนก (4 หลัก)</label><input type="text" id="newDeptCode" placeholder="เช่น 0039" maxlength="4" style="font-family:var(--mo)"></div>
                <div class="fg" style="margin-bottom:10px"><label>ชื่อแผนก *</label><input type="text" id="newDeptName" placeholder="เช่น ICU 2" onkeydown="if(event.key==='Enter')addDept()"></div>
                <button class="btn badm" style="width:100%" onclick="addDept()">➕ เพิ่มแผนก</button>
              </div>
              <div style="background:var(--s2);border-radius:8px;padding:14px" id="editDeptPanel" style="display:none">
                <div style="font-size:11px;font-weight:700;color:#f59e0b;margin-bottom:8px">✏️ แก้ไขแผนกที่เลือก</div>
                <div class="fg" style="margin-bottom:8px"><label>รหัสแผนก</label><input type="text" id="editDeptCode" placeholder="รหัส" maxlength="4" style="font-family:var(--mo)"></div>
                <div class="fg" style="margin-bottom:10px"><label>ชื่อแผนก *</label><input type="text" id="editDeptName" placeholder="ชื่อแผนก"></div>
                <div style="display:flex;gap:6px">
                  <button class="btn badm" style="flex:1" onclick="saveEditDept()">💾 บันทึก</button>
                  <button class="btn bg" onclick="cancelEditDept()">ยกเลิก</button>
                </div>
              </div>
              <div style="margin-top:12px;background:rgba(0,200,255,.05);border:1px solid rgba(0,200,255,.15);border-radius:6px;padding:10px">
                <div style="font-size:11px;font-weight:700;color:var(--ac);margin-bottom:6px">💡 นำเข้าหลายแผนกพร้อมกัน</div>
                <textarea id="bulkDeptInput" rows="4" placeholder="พิมพ์ชื่อแผนกทีละบรรทัด เช่น&#10;ICU 2&#10;Ward 5&#10;ห้องผ่าตัด 2" style="font-size:12px;margin-bottom:6px"></textarea>
                <button class="btn bp sm" onclick="bulkAddDepts()">📥 นำเข้าทั้งหมด</button>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn bg sm" onclick="exportListJSON('dept')">📤 Export JSON</button>
            <button class="btn badm sm" onclick="applyDeptChanges()">✅ บันทึกการเปลี่ยนแปลงทั้งหมด</button>
          </div>
        </div>
      </div>

      <!-- STAFF LIST -->
      <div id="lsec-staff" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">👤 จัดการรายชื่อบุคลากร</div>
            <div style="font-size:11px;color:var(--mu)">ใช้สำหรับ Dropdown "ผู้ให้ยืม" และ "ผู้รับคืน"</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <!-- Left: list -->
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายชื่อบุคลากรปัจจุบัน <span id="staffCountBadge" style="background:rgba(124,58,237,.15);color:var(--ac2);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <input type="text" id="staffFilterInput" placeholder="🔍 กรองรายชื่อ..." oninput="renderStaffList()" style="margin-bottom:8px;font-size:12px">
              <div id="staffListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveStaffItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveStaffItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedStaff()">🗑 ลบที่เลือก</button>
              </div>
            </div>
            <!-- Right: add -->
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่ม / แก้ไขบุคลากร</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:8px"><label>ยศ/คำนำหน้า</label>
                  <select id="newStaffPrefix" style="margin-bottom:6px">
                    <option value="">— ไม่มี —</option>
                    <option value="ร.อ.">ร.อ.</option><option value="ร.ท.">ร.ท.</option><option value="ร.ต.">ร.ต.</option>
                    <option value="น.อ.">น.อ.</option><option value="น.ท.">น.ท.</option><option value="น.ต.">น.ต.</option>
                    <option value="พ.จ.อ.">พ.จ.อ.</option><option value="จ.ส.อ.">จ.ส.อ.</option>
                    <option value="ร.อ.หญิง">ร.อ.หญิง</option><option value="น.ต.หญิง">น.ต.หญิง</option>
                    <option value="น.ท.หญิง">น.ท.หญิง</option><option value="พ.จ.อ.หญิง">พ.จ.อ.หญิง</option>
                    <option value="นาย">นาย</option><option value="นาง">นาง</option>
                    <option value="น.ส.">น.ส.</option><option value="นพ.">นพ.</option><option value="พญ.">พญ.</option>
                  </select>
                </div>
                <div class="fg" style="margin-bottom:10px"><label>ชื่อ-นามสกุล *</label><input type="text" id="newStaffName" placeholder="ชื่อ นามสกุล" onkeydown="if(event.key==='Enter')addStaff()"></div>
                <button class="btn badm" style="width:100%" onclick="addStaff()">➕ เพิ่มบุคลากร</button>
              </div>
              <div style="background:var(--s2);border-radius:8px;padding:14px" id="editStaffPanel">
                <div style="font-size:11px;font-weight:700;color:#f59e0b;margin-bottom:8px">✏️ แก้ไขชื่อที่เลือก</div>
                <div class="fg" style="margin-bottom:10px"><label>ชื่อใหม่</label><input type="text" id="editStaffName" placeholder="ชื่อใหม่"></div>
                <div style="display:flex;gap:6px">
                  <button class="btn badm" style="flex:1" onclick="saveEditStaff()">💾 บันทึก</button>
                  <button class="btn bg" onclick="cancelEditStaff()">ยกเลิก</button>
                </div>
              </div>
              <div style="margin-top:12px;background:rgba(124,58,237,.05);border:1px solid rgba(124,58,237,.2);border-radius:6px;padding:10px">
                <div style="font-size:11px;font-weight:700;color:var(--ac2);margin-bottom:6px">💡 นำเข้าหลายคนพร้อมกัน</div>
                <textarea id="bulkStaffInput" rows="4" placeholder="พิมพ์ชื่อทีละบรรทัด เช่น&#10;ร.อ.สมชาย&#10;น.ส.นิภา&#10;พ.จ.อ.วิชัย" style="font-size:12px;margin-bottom:6px"></textarea>
                <button class="btn bp sm" onclick="bulkAddStaff()">📥 นำเข้าทั้งหมด</button>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;padding:10px;background:rgba(16,185,129,.05);border:1px solid rgba(16,185,129,.2);border-radius:6px">
            <div style="font-size:11px;font-weight:700;color:var(--ok);margin-bottom:6px">⚠️ หมายเหตุ</div>
            <div style="font-size:12px;color:var(--mu)">รายชื่อ "อื่นๆ" จะถูกเพิ่มต่อท้ายเสมอ ไม่สามารถลบได้ | การเปลี่ยนแปลงมีผลทันทีกับ Dropdown ผู้ให้ยืม/ผู้รับคืน/ผู้โอน</div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn bg sm" onclick="exportListJSON('staff')">📤 Export JSON</button>
            <button class="btn badm sm" onclick="applyStaffChanges()">✅ บันทึกการเปลี่ยนแปลงทั้งหมด</button>
          </div>
        </div>
      </div>

      <!-- EQUIPMENT LIST -->
      <div id="lsec-eq" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">🔧 จัดการรายการเครื่องมือแพทย์</div>
            <div style="font-size:11px;color:var(--mu)">ใช้สำหรับการ์ดเลือกเครื่องมือ และ Dropdown filter ทุกแห่ง</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายการปัจจุบัน <span id="eqCountBadge" style="background:rgba(245,158,11,.15);color:var(--wn);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <input type="text" id="eqFilterInput" placeholder="🔍 กรอง..." oninput="renderEqList()" style="margin-bottom:8px;font-size:12px">
              <div id="eqListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveEqItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveEqItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedEq()">🗑 ลบที่เลือก</button>
              </div>
            </div>
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่ม / แก้ไขเครื่องมือ</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:10px"><label>ชื่อเครื่องมือ *</label><input type="text" id="newEqName" placeholder="เช่น High Flow O2" onkeydown="if(event.key==='Enter')addEqItem()"></div>
                <button class="btn badm" style="width:100%" onclick="addEqItem()">➕ เพิ่มรายการ</button>
              </div>
              <div style="background:var(--s2);border-radius:8px;padding:14px" id="editEqPanel">
                <div style="font-size:11px;font-weight:700;color:#f59e0b;margin-bottom:8px">✏️ แก้ไขรายการที่เลือก</div>
                <div class="fg" style="margin-bottom:10px"><label>ชื่อใหม่ *</label><input type="text" id="editEqName" placeholder="ชื่อเครื่องมือใหม่"></div>
                <div style="display:flex;gap:6px">
                  <button class="btn badm" style="flex:1" onclick="saveEditEq()">💾 บันทึก</button>
                  <button class="btn bg" onclick="cancelEditEq()">ยกเลิก</button>
                </div>
              </div>
              <div style="margin-top:12px;background:rgba(239,68,68,.05);border:1px solid rgba(239,68,68,.2);border-radius:6px;padding:10px">
                <div style="font-size:11px;font-weight:700;color:var(--er);margin-bottom:4px">⚠️ คำเตือน</div>
                <div style="font-size:12px;color:var(--mu)">การลบ/เปลี่ยนชื่อเครื่องมือจะ<strong style="color:var(--er)">ไม่</strong>กระทบข้อมูลที่บันทึกไปแล้ว แต่จะไม่แสดงใน filter เก่าได้ถูกต้อง</div>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn bg sm" onclick="exportListJSON('eq')">📤 Export JSON</button>
            <button class="btn badm sm" onclick="applyEqChanges()">✅ บันทึกการเปลี่ยนแปลงทั้งหมด</button>
          </div>
        </div>
      </div>

      <!-- ACCESSORIES LIST -->
      <div id="lsec-acc" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">📦 จัดการอุปกรณ์ประกอบ</div>
            <div style="font-size:11px;color:var(--mu)">รายการที่แสดงในตาราง "อุปกรณ์ประกอบ" ตอนบันทึกการยืม</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายการปัจจุบัน <span id="accCountBadge" style="background:rgba(16,185,129,.12);color:var(--ok);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <div id="accListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveAccItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveAccItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedAcc()">🗑 ลบที่เลือก</button>
              </div>
            </div>
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่ม / แก้ไขอุปกรณ์</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:8px"><label>ชื่ออุปกรณ์ *</label><input type="text" id="newAccName" placeholder="เช่น Nasal cannula adult"></div>
                <div class="fg" style="margin-bottom:8px"><label>ประเภทช่องกรอก</label>
                  <select id="newAccType">
                    <option value="no">มีหมายเลข (No.) — กรอกเป็นตัวเลข</option>
                    <option value="det">มีรายละเอียด — กรอกเป็นข้อความ</option>
                    <option value="none">ไม่มีช่องกรอก (tick เท่านั้น)</option>
                  </select>
                </div>
                <button class="btn badm" style="width:100%;margin-top:4px" onclick="addAccItem()">➕ เพิ่มอุปกรณ์</button>
              </div>
              <div style="background:var(--s2);border-radius:8px;padding:14px" id="editAccPanel">
                <div style="font-size:11px;font-weight:700;color:#f59e0b;margin-bottom:8px">✏️ แก้ไขอุปกรณ์ที่เลือก</div>
                <div class="fg" style="margin-bottom:8px"><label>ชื่อใหม่ *</label><input type="text" id="editAccName" placeholder="ชื่ออุปกรณ์"></div>
                <div class="fg" style="margin-bottom:10px"><label>ประเภทช่องกรอก</label>
                  <select id="editAccType">
                    <option value="no">มีหมายเลข (No.)</option>
                    <option value="det">มีรายละเอียด (ข้อความ)</option>
                    <option value="none">ไม่มีช่องกรอก</option>
                  </select>
                </div>
                <div style="display:flex;gap:6px">
                  <button class="btn badm" style="flex:1" onclick="saveEditAcc()">💾 บันทึก</button>
                  <button class="btn bg" onclick="cancelEditAcc()">ยกเลิก</button>
                </div>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn bg sm" onclick="exportListJSON('acc')">📤 Export JSON</button>
            <button class="btn badm sm" onclick="applyAccChanges()">✅ บันทึกการเปลี่ยนแปลงทั้งหมด</button>
          </div>
        </div>
      </div>


      <!-- EQNUM LIST -->
      <div id="lsec-eqnum" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">🔢 จัดการหมายเลขเครื่องมือ</div>
            <div style="font-size:11px;color:var(--mu)">ใช้สำหรับ Dropdown หมายเลขเครื่องมือในหน้าบันทึกการยืม</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายการปัจจุบัน <span id="eqnumCountBadge" style="background:rgba(0,200,255,.15);color:var(--ac);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <div id="eqnumListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveEqnumItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveEqnumItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedEqnum()">🗑 ลบ</button>
              </div>
            </div>
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่มหมายเลขใหม่</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:10px"><label>หมายเลขเครื่องมือ *</label>
                  <input type="text" id="newEqnumName" placeholder="เช่น O2FLO4, MEK6 ..." onkeydown="if(event.key==='Enter')addEqnumItem()">
                </div>
                <button class="btn badm" style="width:100%" onclick="addEqnumItem()">➕ เพิ่มหมายเลข</button>
              </div>
              <div style="margin-top:12px;background:rgba(0,200,255,.05);border:1px solid rgba(0,200,255,.15);border-radius:6px;padding:10px">
                <div style="font-size:11px;font-weight:700;color:var(--ac);margin-bottom:6px">💡 นำเข้าหลายรายการพร้อมกัน</div>
                <textarea id="bulkEqnumInput" rows="4" placeholder="พิมพ์หมายเลขทีละบรรทัด เช่น&#10;O2FLO4&#10;MEK6&#10;VEN1" style="font-size:12px;margin-bottom:6px"></textarea>
                <button class="btn bp sm" onclick="bulkAddEqnums()">📥 นำเข้าทั้งหมด</button>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn badm sm" onclick="applyEqnumChanges()">✅ บันทึกการเปลี่ยนแปลง</button>
          </div>
        </div>
      </div>

      <!-- RETREM LIST -->
      <div id="lsec-retrem" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">↩ จัดการเหตุผลการรับคืน</div>
            <div style="font-size:11px;color:var(--mu)">ใช้สำหรับ Dropdown เหตุผลการคืนในหน้าบันทึกการคืน</div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px">
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">รายการปัจจุบัน <span id="retremCountBadge" style="background:rgba(16,185,129,.15);color:var(--ok);border-radius:10px;padding:1px 8px;font-size:11px;font-weight:700"></span></div>
              <div id="retremListArea" style="max-height:420px;overflow-y:auto;border:1px solid var(--bd);border-radius:6px"></div>
              <div style="margin-top:8px;display:flex;gap:6px;flex-wrap:wrap">
                <button class="btn bg sm" onclick="moveRetremItem(-1)">⬆ ขึ้น</button>
                <button class="btn bg sm" onclick="moveRetremItem(1)">⬇ ลง</button>
                <button class="btn ber sm" onclick="deleteSelectedRetrem()">🗑 ลบ</button>
              </div>
            </div>
            <div>
              <div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:8px">เพิ่มเหตุผลใหม่</div>
              <div style="background:var(--s2);border-radius:8px;padding:14px;margin-bottom:10px">
                <div class="fg" style="margin-bottom:8px"><label>ข้อความที่แสดง *</label>
                  <input type="text" id="newRetremLabel" placeholder="เช่น ย้ายไป ICU">
                </div>
                <div class="fg" style="margin-bottom:10px"><label>ต้องกรอกรายละเอียดเพิ่ม?</label>
                  <select id="newRetremDetail">
                    <option value="0">ไม่ต้อง</option>
                    <option value="1">ต้องกรอก</option>
                  </select>
                </div>
                <button class="btn badm" style="width:100%" onclick="addRetremItem()">➕ เพิ่มเหตุผล</button>
              </div>
              <div style="margin-top:8px;padding:8px 10px;background:rgba(16,185,129,.05);border:1px solid rgba(16,185,129,.2);border-radius:6px;font-size:11px;color:var(--mu)">
                ⚠️ รายการ "อื่นๆ" จะแสดงช่องกรอกรายละเอียดเสมอ ไม่สามารถลบได้
              </div>
            </div>
          </div>
          <div style="margin-top:12px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn badm sm" onclick="applyRetremChanges()">✅ บันทึกการเปลี่ยนแปลง</button>
          </div>
        </div>
      </div>


      <!-- HFNC SET MANAGER -->
      <div id="lsec-hfncset" style="display:none">
        <div class="card">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">
            <div class="ctitle" style="margin:0;color:#f59e0b">💨 จัดการ HFNC Set Mapping</div>
            <div style="font-size:11px;color:var(--mu)">กำหนดช่วงหมายเลขเครื่องของแต่ละ set เพื่อใช้ในหน้าสถิติ</div>
          </div>
          <div id="hfncSetAdminArea"></div>
          <div style="margin-top:14px;display:flex;gap:6px;justify-content:flex-end">
            <button class="btn badm sm" onclick="applyHfncSetChanges()">✅ บันทึกการเปลี่ยนแปลง</button>
          </div>
        </div>
      </div>

    </div><!-- /adm-lists-mgr -->


    <!-- ── TAB: PIN MANAGER ── -->
    <div class="admin-sub" id="adm-pin-mgr">
      <div class="card" style="max-width:560px">
        <div class="ctitle" style="color:#f59e0b;margin-bottom:16px">🔑 จัดการรหัสผ่าน</div>

        <!-- App PIN -->
        <div style="background:var(--s2);border:1px solid var(--bd);border-radius:10px;padding:16px 18px;margin-bottom:14px;border-left:4px solid var(--ac)">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
            <span style="font-size:18px">📋</span>
            <div>
              <div style="font-size:14px;font-weight:700;color:var(--tx)">รหัสผ่านระบบ (App PIN)</div>
              <div style="font-size:11px;color:var(--mu)">สำหรับ จนท.แผนกบริการเครื่องมือฯ ใช้บันทึกยืม-คืน-โอน</div>
            </div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:10px">
            <div>
              <label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">รหัสปัจจุบัน</label>
              <div style="display:flex;gap:6px;align-items:center">
                <input type="password" id="showAppPin" value="" placeholder="••••" readonly
                  style="flex:1;font-size:18px;letter-spacing:6px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
                <button class="abt" onclick="togglePinShow('showAppPin',this)" style="font-size:12px;padding:6px 10px">👁</button>
              </div>
            </div>
            <div>
              <label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">รหัสใหม่</label>
              <input type="password" id="newAppPin" placeholder="กรอกรหัสใหม่" maxlength="20"
                style="width:100%;font-size:15px;letter-spacing:4px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
            </div>
          </div>
          <div style="display:flex;gap:8px;align-items:center">
            <input type="password" id="newAppPinConfirm" placeholder="ยืนยันรหัสใหม่" maxlength="20"
              style="flex:1;font-size:15px;letter-spacing:4px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
            <button class="btn bg sm" onclick="changePIN('app')" style="white-space:nowrap">💾 บันทึก</button>
          </div>
          <div id="appPinMsg" style="font-size:12px;margin-top:8px;min-height:16px"></div>
        </div>

        <!-- Admin PIN -->
        <div style="background:var(--s2);border:1px solid var(--bd);border-radius:10px;padding:16px 18px;margin-bottom:14px;border-left:4px solid #f59e0b">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
            <span style="font-size:18px">⚙️</span>
            <div>
              <div style="font-size:14px;font-weight:700;color:var(--tx)">รหัสผ่าน Admin</div>
              <div style="font-size:11px;color:var(--mu)">สำหรับเข้าหน้า Admin Panel จัดการข้อมูลทั้งหมด</div>
            </div>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:10px">
            <div>
              <label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">รหัสปัจจุบัน</label>
              <div style="display:flex;gap:6px;align-items:center">
                <input type="password" id="showAdmPin" value="" placeholder="••••" readonly
                  style="flex:1;font-size:18px;letter-spacing:6px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
                <button class="abt" onclick="togglePinShow('showAdmPin',this)" style="font-size:12px;padding:6px 10px">👁</button>
              </div>
            </div>
            <div>
              <label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">รหัสใหม่</label>
              <input type="password" id="newAdmPin" placeholder="กรอกรหัสใหม่" maxlength="20"
                style="width:100%;font-size:15px;letter-spacing:4px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
            </div>
          </div>
          <div style="display:flex;gap:8px;align-items:center">
            <input type="password" id="newAdmPinConfirm" placeholder="ยืนยันรหัสใหม่" maxlength="20"
              style="flex:1;font-size:15px;letter-spacing:4px;text-align:center;padding:8px;background:var(--bg);border:1px solid var(--bd);border-radius:6px;color:var(--tx)">
            <button class="btn badm sm" onclick="changePIN('admin')" style="white-space:nowrap">💾 บันทึก</button>
          </div>
          <div id="admPinMsg" style="font-size:12px;margin-top:8px;min-height:16px"></div>
        </div>

        <!-- Note -->
        <div style="background:rgba(245,158,11,.06);border:1px solid rgba(245,158,11,.2);border-radius:8px;padding:10px 14px;font-size:12px;color:var(--mu)">
          ⚠️ รหัสผ่านจะถูกบันทึกใน localStorage ของเบราว์เซอร์นี้เท่านั้น<br>
          หากล้างข้อมูลเบราว์เซอร์ ระบบจะกลับไปใช้รหัสค่าเริ่มต้น (App: <span id="defaultAppPin" style="font-family:var(--mo);color:var(--ac)">4321</span> / Admin: <span id="defaultAdmPin" style="font-family:var(--mo);color:#f59e0b">9999</span>)
        </div>
      </div>
    </div>

  </div><!-- /adminContent -->
</div><!-- /pg-admin -->

<!-- ══════════════════════════════════════════════════
     📖 คู่มือ PAGE
══════════════════════════════════════════════════ -->
<div class="pg" id="pg-manual" style="padding:14px;max-width:960px;margin:0 auto">

<!-- header bar -->
<div style="background:var(--s1);border:1px solid var(--bd);border-radius:10px;padding:14px 18px;margin-bottom:12px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:10px">
  <div>
    <div style="font-size:15px;font-weight:700;color:var(--ac);display:flex;align-items:center;gap:7px">📖 คู่มือการใช้งานระบบยืมคืนเครื่องมือแพทย์</div>
    <div style="font-size:11px;color:var(--mu);margin-top:2px">อ่านออนไลน์ หรือดาวน์โหลดเพื่อใช้งานออฟไลน์</div>
  </div>
  <div style="display:flex;gap:6px;flex-wrap:wrap;align-items:center">
    <span style="font-size:10px;color:var(--mu);font-weight:700;text-transform:uppercase;letter-spacing:.4px">ดาวน์โหลด:</span>
    <button class="btn bg sm" onclick="manDL('word')" title="Word Document">📄 Word</button>
    <button class="btn bg sm" onclick="manDL('pdf')"  title="PDF">🖨️ PDF</button>
    <button class="btn bg sm" onclick="manDL('html')" title="HTML Infographic">🌐 HTML</button>
    <button class="btn bg sm" onclick="manDL('csv')"  title="CSV">📊 CSV</button>
    <button class="btn bg sm" onclick="manDL('xls')"  title="XLS (Excel 97-2003)">📗 XLS</button>
    <button class="btn bg sm" onclick="manDL('xlsx')" title="XLSX (Excel)">📗 XLSX</button>
  </div>
</div>

<!-- tabs -->
<div style="display:flex;gap:3px;margin-bottom:12px;border-bottom:1px solid var(--bd)">
  <button id="mtab-info"   onclick="showMTab('info',this)"   class="abt" style="border-radius:6px 6px 0 0;border-bottom:none;font-size:12px;font-weight:700;padding:7px 16px;background:rgba(0,200,255,.12);color:var(--ac);border-color:var(--bd)">🎨 อินโฟกราฟิก</button>
  <button id="mtab-word"   onclick="showMTab('word',this)"   class="abt" style="border-radius:6px 6px 0 0;border-bottom:none;font-size:12px;font-weight:600;padding:7px 16px">📋 คู่มือฉบับเต็ม</button>
</div>

<!-- tab: infographic -->
<div id="mcont-info" style="display:block">
  <div id="infographic-host" style="background:#0a0f1e;border-radius:10px;overflow:hidden;border:1px solid rgba(0,200,255,.15)">
<!-- ══ HERO ════════════════════════════════════════ -->
<div class="hero">
  <div class="hero-pill">🏥 คู่มือการใช้งาน</div>
  <h1>ระบบยืมคืนเครื่องมือแพทย์</h1>
  <p class="sub">Medical Equipment Borrowing System — คู่มือฉบับย่อ ใช้งานได้ทันที</p>
  <div class="hero-stats">
    <div class="hero-stat"><div class="n">7</div><div class="l">เมนูหลัก</div></div>
    <div class="hero-stat"><div class="n">5</div><div class="l">สถานะ</div></div>
    <div class="hero-stat"><div class="n">3</div><div class="l">ระดับผู้ใช้</div></div>
    <div class="hero-stat"><div class="n">Real-time</div><div class="l">ข้อมูล</div></div>
  </div>
</div>

<!-- ══ SECTION 1: ผู้ใช้งาน ══════════════════════ -->
<div class="section">
  <div class="sec-title">ผู้ใช้งาน</div>
  <div class="roles">
    <div class="role-card r1">
      <div class="role-icon">🏥</div>
      <div class="role-name">แผนกผู้ยืม</div>
      <div class="role-desc">กรอกแบบฟอร์มแจ้งยืมเครื่องมือสำหรับผู้ป่วย</div>
      <div class="role-badge">ไม่ต้องมีรหัสผ่าน</div>
    </div>
    <div class="role-card r2">
      <div class="role-icon">🔧</div>
      <div class="role-name">จนท.บริการเครื่องมือฯ</div>
      <div class="role-desc">บันทึกการจ่าย คืน และโอนเครื่องมือ</div>
      <div class="role-badge">ต้องใช้รหัสผ่าน</div>
    </div>
    <div class="role-card r3">
      <div class="role-icon">⚙️</div>
      <div class="role-name">Admin</div>
      <div class="role-desc">จัดการข้อมูลทั้งหมด ตั้งค่า แก้ไข ลบ</div>
      <div class="role-badge">ต้องใช้รหัส Admin</div>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 2: แท็บเมนู ═══════════════════════ -->
<div class="section">
  <div class="sec-title">แท็บเมนูหลัก</div>
  <div class="tabs-grid">
    <div class="tab-card">
      <div class="tab-emoji">📋</div>
      <div class="tab-info">
        <div class="tn">ลงบันทึกแจ้งยืม</div>
        <div class="td">แผนกกรอกข้อมูลขอยืมเครื่องมือ ระบุผู้ป่วยและเลือกชนิดเครื่องมือ</div>
      </div>
    </div>
    <div class="tab-card">
      <div class="tab-emoji">📝</div>
      <div class="tab-info">
        <div class="tn">ลงบันทึกยืม-คืน-โอน</div>
        <div class="td">จนท.จัดการรายการ บันทึกจ่าย รับคืน และโอนเครื่องมือ</div>
      </div>
    </div>
    <div class="tab-card">
      <div class="tab-emoji">📅</div>
      <div class="tab-info">
        <div class="tn">สรุปรายการประจำวัน</div>
        <div class="td">ดูรายการที่กำลังยืมอยู่ทั้งหมด เรียงตามชนิดเครื่องมือ</div>
      </div>
    </div>
    <div class="tab-card">
      <div class="tab-emoji">📊</div>
      <div class="tab-info">
        <div class="tn">Dashboard</div>
        <div class="td">ภาพรวม Real-time สถิติ จำนวนยืม-คืน แจ้งเตือน HFNC</div>
      </div>
    </div>
    <div class="tab-card">
      <div class="tab-emoji">🗂</div>
      <div class="tab-info">
        <div class="tn">ข้อมูลทั้งหมด</div>
        <div class="td">ค้นหาและดูประวัติการยืมทุกรายการในระบบ</div>
      </div>
    </div>
    <div class="tab-card">
      <div class="tab-emoji">📈</div>
      <div class="tab-info">
        <div class="tn">สถิติ</div>
        <div class="td">กราฟ ค่าเฉลี่ย ยืมเกิน 30 วัน Circuit HFNC set</div>
      </div>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 3: WORKFLOW ═══════════════════════ -->
<div class="section">
  <div class="sec-title">ขั้นตอนการใช้งาน</div>
  <div class="workflow">

    <div class="wf-step">
      <div class="wf-num c1">1</div>
      <div class="wf-body">
        <div class="wt">กรอกแบบฟอร์มแจ้งยืม</div>
        <div class="wd">กรอก วันที่ยืม / ชื่อผู้ยืม / แผนก / ชื่อผู้ป่วย แล้วคลิกการ์ดเครื่องมือที่ต้องการ กด "📤 ส่งแบบฟอร์มยืม" — ได้รับ LoanID</div>
        <span class="who" style="color:var(--ac1)">👤 แผนกผู้ยืม</span>
      </div>
    </div>

    <div class="wf-step">
      <div class="wf-num c2">2</div>
      <div class="wf-body">
        <div class="wt">บันทึกการจ่ายเครื่องมือ</div>
        <div class="wd">เปิดแท็บ "📝 ลงบันทึกยืม-คืน-โอน" → กดปุ่ม "📝 บันทึก" → เลือกผู้ให้ยืม กรอกหมายเลขเครื่อง เลือกอุปกรณ์ประกอบ → กด "💾 บันทึก"</div>
        <span class="who" style="color:var(--ac3)">🔧 จนท.บริการเครื่องมือฯ</span>
      </div>
    </div>

    <div class="wf-step">
      <div class="wf-num c3">3</div>
      <div class="wf-body">
        <div class="wt">โอนเครื่องมือ (ถ้ามี)</div>
        <div class="wd">กดปุ่ม "🔄 โอน" → เลือกผู้โอน → เลือกแผนกปลายทาง → กด "🔄 โอน" — ระบบบันทึก breadcrumb trail การโอนทั้งหมดไว้</div>
        <span class="who" style="color:var(--ac2)">🔧 จนท.บริการเครื่องมือฯ</span>
      </div>
    </div>

    <div class="wf-step">
      <div class="wf-num c4">4</div>
      <div class="wf-body">
        <div class="wt">บันทึกการคืน</div>
        <div class="wd">กดปุ่ม "↩ คืน" → ติ๊กตัวเครื่องและอุปกรณ์ประกอบที่คืน → เลือกผู้รับคืน เหตุผล วันที่ → กด "✅ บันทึกการคืน"</div>
        <span class="who" style="color:var(--ac4)">🔧 จนท.บริการเครื่องมือฯ</span>
      </div>
    </div>

    <div class="wf-step">
      <div class="wf-num c5">5</div>
      <div class="wf-body">
        <div class="wt">คืนอุปกรณ์ที่ค้าง (ถ้ามี)</div>
        <div class="wd">หากคืนตัวเครื่องแล้วแต่อุปกรณ์ยังค้าง กดปุ่ม "📦 คืนอุปกรณ์" แล้วติ๊กรายการที่นำมาคืนในครั้งนี้</div>
        <span class="who" style="color:var(--ac5)">🔧 จนท.บริการเครื่องมือฯ</span>
      </div>
    </div>

  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 4: สถานะ ═══════════════════════════ -->
<div class="section">
  <div class="sec-title">สถานะของรายการ</div>
  <div class="status-grid">
    <div class="status-card">
      <div class="status-dot" style="background:#f59e0b;box-shadow:0 0 6px rgba(245,158,11,.5)"></div>
      <div class="status-info">
        <div class="sn">รอดำเนินการจ่าย</div>
        <div class="sd">ส่งแบบฟอร์มแล้ว รอ จนท.จัดการ</div>
        <div class="sact">→ จนท.กดบันทึก</div>
      </div>
    </div>
    <div class="status-card">
      <div class="status-dot" style="background:#10b981;box-shadow:0 0 6px rgba(16,185,129,.5)"></div>
      <div class="status-info">
        <div class="sn">ยืม</div>
        <div class="sd">กำลังใช้งานอยู่</div>
        <div class="sact">→ นำมาคืนเมื่อเสร็จ</div>
      </div>
    </div>
    <div class="status-card">
      <div class="status-dot" style="background:#f59e0b;box-shadow:0 0 6px rgba(245,158,11,.5)"></div>
      <div class="status-info">
        <div class="sn">คืน (อุปกรณ์ค้าง)</div>
        <div class="sd">คืนตัวเครื่องแล้ว แต่อุปกรณ์ยังค้าง</div>
        <div class="sact">→ กด 📦 คืนอุปกรณ์</div>
      </div>
    </div>
    <div class="status-card">
      <div class="status-dot" style="background:#8892aa;box-shadow:0 0 6px rgba(136,146,170,.4)"></div>
      <div class="status-info">
        <div class="sn">คืน</div>
        <div class="sd">คืนครบทุกรายการเรียบร้อย</div>
        <div class="sact">→ เสร็จสิ้น</div>
      </div>
    </div>
    <div class="status-card" style="grid-column:span 2">
      <div class="status-dot" style="background:#ef4444;box-shadow:0 0 6px rgba(239,68,68,.5)"></div>
      <div class="status-info">
        <div class="sn">ยกเลิก</div>
        <div class="sd">รายการถูกยกเลิก — กดปุ่ม "🗑 ลบ" เพื่อลบออกจากระบบ หรือสร้างใบยืมใหม่หากต้องการ</div>
      </div>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 5: แจ้งเตือน HFNC ═════════════════ -->
<div class="section">
  <div class="sec-title">แจ้งเตือน HFNC</div>
  <div class="alert-box">
    <div class="alert-icon">⚠️</div>
    <div class="alert-content">
      <div class="al">ระบบแจ้งเตือนอัตโนมัติเมื่อ HFNC เกินเกณฑ์</div>
      <div class="ad">ตรวจสอบได้ที่แท็บ 📊 Dashboard — ระบบแสดงแถบสีแดงเมื่อเกินเกณฑ์ พร้อมแสดงรายละเอียดแยกตามอาคารและหอผู้ป่วย</div>
      <div class="alert-limits">
        <div class="alert-limit">
          <div class="ln">30</div>
          <div class="ll">อาคาร 100 ปี</div>
        </div>
        <div class="alert-limit">
          <div class="ln">65</div>
          <div class="ll">รวมทุกอาคาร</div>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 6: Admin ═══════════════════════════ -->
<div class="section">
  <div class="sec-title">Admin Panel</div>
  <div class="admin-grid">
    <div class="admin-card">
      <div class="at">📋 ใบยืม &amp; บันทึกยืม</div>
      <div class="ad">แก้ไข เพิ่ม ลบ เปลี่ยนสถานะ Bulk select</div>
    </div>
    <div class="admin-card">
      <div class="at">⚡ แก้ไขด่วน</div>
      <div class="ad">เปลี่ยนสถานะ วันที่ แผนก หมายเลขเครื่อง</div>
    </div>
    <div class="admin-card">
      <div class="at">🗃 จัดการรายการ</div>
      <div class="ad">แผนก บุคลากร เครื่องมือ อุปกรณ์ หมายเลข เหตุผลคืน HFNC set</div>
    </div>
    <div class="admin-card">
      <div class="at">📊 สถานะเครื่องมือ</div>
      <div class="ad">ตั้งค่า Total / Inactive / For Dept Use ของแต่ละชนิดเครื่องมือ</div>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ══ SECTION 7: เคล็ดลับ ══════════════════════ -->
<div class="section">
  <div class="sec-title">เคล็ดลับ &amp; FAQ</div>
  <div class="tips-grid">
    <div class="tip-row">
      <div class="tip-icon">🔍</div>
      <div class="tip-txt">
        <div class="tl">ค้นหาเร็วที่สุด</div>
        <div class="td">ใช้หมายเลขเครื่อง (เช่น MEK1, VEN5) หรือ LoanID ค้นหาได้ตรงทุกหน้า</div>
      </div>
    </div>
    <div class="tip-row">
      <div class="tip-icon">⚡</div>
      <div class="tip-txt">
        <div class="tl">หมายเลขเครื่องซ้ำ</div>
        <div class="td">ระบบป้องกันอัตโนมัติ ตรวจสอบจากหน้า "📅 สรุปรายการประจำวัน"</div>
      </div>
    </div>
    <div class="tip-row">
      <div class="tip-icon">🔄</div>
      <div class="tip-txt">
        <div class="tl">ข้อมูลไม่อัปเดต</div>
        <div class="td">กด "🔄 โหลด" หรือ "🔄 Refresh" — ข้อมูลจะ sync อัตโนมัติทุก 5 นาที</div>
      </div>
    </div>
    <div class="tip-row">
      <div class="tip-icon">📥</div>
      <div class="tip-txt">
        <div class="tl">ดาวน์โหลดรายงาน</div>
        <div class="td">ทุกหน้ารองรับ CSV / XLS / XLSX / PDF กรอง Filter ก่อนดาวน์โหลดเพื่อรับเฉพาะข้อมูลที่ต้องการ</div>
      </div>
    </div>
    <div class="tip-row">
      <div class="tip-icon">✏️</div>
      <div class="tip-txt">
        <div class="tl">แก้ไขข้อมูลที่บันทึกผิด</div>
        <div class="td">ติดต่อ Admin → Admin Panel → ⚡ แก้ไขด่วน — แก้ไขได้ทุกช่อง</div>
      </div>
    </div>
    <div class="tip-row">
      <div class="tip-icon">📊</div>
      <div class="tip-txt">
        <div class="tl">ตรวจสอบประจำวัน</div>
        <div class="td">เปิดแท็บ "📅 สรุปรายการประจำวัน" — ดูรายการสีแดง (>30 วัน) เพื่อติดตามคืน</div>
      </div>
    </div>
  </div>
</div>

<!-- ══ FOOTER ═════════════════════════════════════ -->
<div class="footer">
  แผนกบริการเครื่องมือและอุปกรณ์ทางการแพทย์
  <div class="fv">เวอร์ชัน 1.0 · ปี 2569 · Real-time via Supabase</div>
</div>
  </div>
</div>

<!-- tab: word viewer -->
<div id="mcont-word" style="display:none">
  <div id="wv-wrap" style="background:var(--s1);border:1px solid var(--bd);border-radius:10px;padding:28px 36px;line-height:1.9;font-size:14px">
    <div id="wv-status" style="text-align:center;padding:30px;color:var(--mu)">
      <span id="wv-spin" style="display:inline-block;width:18px;height:18px;border:2px solid var(--bd);border-top-color:var(--ac);border-radius:50%;animation:spn .8s linear infinite;vertical-align:middle;margin-right:8px"></span>
      กำลังโหลดคู่มือ...
    </div>
    <div id="wv-body" style="display:none"></div>
  </div>
</div>

</div><!-- /pg-manual -->


<!-- MODAL: BORROW -->
<div class="mbk" id="mBorrow">
  <div class="mdl">
    <div class="mhd"><div class="mttl">📝 บันทึกการยืมเครื่องมือ</div><button class="mcls" onclick="closeM('mBorrow')">✕</button></div>
    <div class="mbdy">
      <input type="hidden" id="bLoanID"><input type="hidden" id="bIdx">
      <div class="fg" style="margin-bottom:9px"><label>ข้อมูลใบยืม</label>
        <div id="bInfo" style="background:var(--s2);border-radius:5px;padding:9px;font-size:13px;line-height:1.8"></div></div>
      <div style="background:rgba(0,200,255,.05);border:1px solid rgba(0,200,255,.18);border-radius:5px;padding:9px;margin-bottom:10px">
        <div style="font-size:10px;font-weight:700;color:var(--ac);margin-bottom:3px;text-transform:uppercase;letter-spacing:.5px">🔧 รายการที่กำลังบันทึก</div>
        <div id="bItem" style="font-size:14px;font-weight:600"></div>
      </div>
      <div class="fgrid" style="margin-bottom:10px">
        <div class="fg"><label>ผู้ให้ยืม *</label>
          <select id="giverSel" onchange="handleStaff('giver')"><option value="">— เลือกผู้ให้ยืม —</option></select>
          <input type="text" id="giverOth" class="sth" placeholder="ระบุชื่อ..."></div>
        <div class="fg"><label>หมายเลขเครื่องมือ</label>
          <div style="position:relative">
            <input type="text" id="eqNo" list="eqNoList" placeholder="กรอกหรือเลือกหมายเลข..." autocomplete="off" style="width:100%">
            <datalist id="eqNoList"></datalist>
          </div>
        </div>
      </div>
      <label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:5px">อุปกรณ์ประกอบ</label>
      <table style="width:100%;border-collapse:collapse"><thead><tr>
        <th style="font-size:10px;color:var(--mu);padding:4px 7px;border-bottom:1px solid var(--bd);text-align:left">เลือก</th>
        <th style="font-size:10px;color:var(--mu);padding:4px 7px;border-bottom:1px solid var(--bd);text-align:left">อุปกรณ์</th>
        <th style="font-size:10px;color:var(--mu);padding:4px 7px;border-bottom:1px solid var(--bd);text-align:left">No. / รายละเอียด</th>
      </tr></thead><tbody id="accBody"></tbody></table>
      <div style="margin-top:6px"><label style="font-size:10px;color:var(--mu);display:block;margin-bottom:3px">รายการที่เลือก:</label>
        <div id="accTags" style="display:flex;flex-wrap:wrap;gap:3px;min-height:22px"></div></div>
    </div>
    <div class="mftr">
      <button class="btn bg" onclick="closeM('mBorrow')">ยกเลิก</button>
      <button class="btn bp" id="saveBBtn" onclick="saveBorrow()">💾 บันทึก</button>
    </div>
  </div>
</div>

<!-- MODAL: RETURN -->
<div class="mbk" id="mReturn">
  <div class="mdl" style="max-width:620px">
    <div class="mhd"><div class="mttl">↩ บันทึกการคืนเครื่องมือ</div><button class="mcls" onclick="closeM('mReturn')">✕</button></div>
    <div class="mbdy">
      <input type="hidden" id="retRecID">
      <div id="retInfo" style="background:var(--s2);border-radius:6px;padding:9px;font-size:13px;margin-bottom:11px;line-height:1.8"></div>
      <div id="retMachineSection" style="margin-bottom:11px">
        <label style="font-size:10px;font-weight:700;color:var(--ac);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:6px">คืนตัวเครื่อง</label>
        <div style="background:var(--s2);border-radius:6px;padding:10px 12px">
          <div style="display:flex;align-items:center;gap:10px">
            <input type="checkbox" id="retMachineCb" style="width:20px;height:20px;accent-color:var(--ok);cursor:pointer;flex-shrink:0" onchange="retUpdateStatus()">
            <label for="retMachineCb" id="retMachineLbl" style="font-size:14px;font-weight:600;cursor:pointer;flex:1;color:var(--tx)">คืนตัวเครื่อง</label>
            <span id="retMachineStatus" style="font-size:11px;color:var(--er)">⏳ ยังไม่คืน</span>
          </div>
        </div>
      </div>
      <div id="retAccSection" style="display:none;margin-bottom:11px">
        <label style="font-size:10px;font-weight:700;color:var(--ac);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:6px">อุปกรณ์ประกอบที่ต้องคืน</label>
        <div id="retAccList" style="background:var(--s2);border-radius:6px;padding:4px 10px"></div>
        <div style="margin-top:6px;font-size:11px;color:var(--mu)" id="retAccSummary"></div>
      </div>
      <div class="fg" style="margin-bottom:9px"><label>ผู้รับคืน *</label>
        <select id="receiverSel" onchange="handleStaff('receiver')"><option value="">— เลือกผู้รับคืน —</option></select>
        <input type="text" id="receiverOth" class="sth" placeholder="ระบุชื่อ..."></div>
      <div class="fg" style="margin-bottom:9px"><label>เหตุผลการคืน *</label>
        <select id="retRemark" onchange="showRetDet()"><option value="">เลือกเหตุผล</option></select></div>
      <div class="fg" id="retDetGrp" style="display:none;margin-bottom:9px">
        <label>รายละเอียดเพิ่มเติม *</label>
        <textarea id="retDet" rows="2" placeholder="ระบุรายละเอียด..."></textarea></div>
      <div class="fg" style="margin-bottom:10px"><label>วันที่รับคืน *</label><input type="date" id="retDate"></div>
      <div id="retStatusBox" style="background:var(--s2);border-radius:6px;padding:9px;font-size:13px;border:1px solid var(--bd)"></div>
    </div>
    <div class="mftr">
      <button class="btn bg" onclick="closeM('mReturn')">ยกเลิก</button>
      <button class="btn bok" onclick="saveReturn()">✅ บันทึกการคืน</button>
    </div>
  </div>
</div>

<!-- MODAL: TRANSFER -->
<div class="mbk" id="mTransfer">
  <div class="mdl mdsm">
    <div class="mhd"><div class="mttl">🔄 โอนเครื่องมือ</div><button class="mcls" onclick="closeM('mTransfer')">✕</button></div>
    <div class="mbdy">
      <input type="hidden" id="trRecID">
      <div id="trInfo" style="background:var(--s2);border-radius:5px;padding:8px;font-size:13px;margin-bottom:10px"></div>
      <div class="fg" style="margin-bottom:9px"><label>ผู้โอน *</label>
        <select id="transfererSel" onchange="handleStaff('transferer')"><option value="">— เลือกผู้โอน —</option></select>
        <input type="text" id="transfererOth" class="sth" placeholder="ระบุชื่อ..."></div>
      <div class="fg" style="margin-bottom:9px"><label>วันที่โอน</label><input type="date" id="trDate"></div>
      <div class="fg" style="margin-bottom:9px"><label>โอนไปแผนก *</label>
        <div class="ssw"><input type="text" id="trDeptSearch" placeholder="พิมพ์แผนก..." oninput="filterSSL('trDeptList',this.value)" onfocus="openSSL('trDeptList')" autocomplete="off">
          <div class="ssl" id="trDeptList"></div></div>
        <input type="hidden" id="trDeptCode"><input type="hidden" id="trDeptName">
        <div class="seld" id="trDeptDisp"></div></div>
      <div class="fg"><label>หมายเหตุ</label><input type="text" id="trNote"></div>
    </div>
    <div class="mftr">
      <button class="btn bg" onclick="closeM('mTransfer')">ยกเลิก</button>
      <button class="btn bwn" onclick="saveTransfer()">🔄 โอน</button>
    </div>
  </div>
</div>

<!-- MODAL: DETAIL -->
<div class="mbk" id="mDetail">
  <div class="mdl">
    <div class="mhd"><div class="mttl">🔍 รายละเอียด</div><button class="mcls" onclick="closeM('mDetail')">✕</button></div>
    <div class="mbdy" id="detBody"></div>
    <div class="mftr"><button class="btn bg" onclick="closeM('mDetail')">ปิด</button></div>
  </div>
</div>

<!-- MODAL: ADMIN EDIT LOAN -->
<div class="mbk" id="mAdmEditLoan">
  <div class="mdl" style="max-width:650px">
    <div class="mhd"><div class="mttl" style="color:#f59e0b">✏️ Admin: แก้ไขใบยืม</div><button class="mcls" onclick="closeM('mAdmEditLoan')">✕</button></div>
    <div class="mbdy">
      <input type="hidden" id="admEditLoanID">
      <div style="background:rgba(245,158,11,.07);border:1px solid rgba(245,158,11,.25);border-radius:6px;padding:8px 12px;margin-bottom:12px;font-size:12px;color:var(--mu)">
        ⚠️ การแก้ไขจะบันทึกลง Supabase ทันที กรุณาตรวจสอบข้อมูลก่อนบันทึก
      </div>
      <div class="fgrid">
        <div class="fg"><label>LoanID (ไม่สามารถแก้ไข)</label><input type="text" id="admEditLoanIDDisp" disabled style="opacity:.5"></div>
        <div class="fg"><label>วันที่ยืม *</label><input type="date" id="admEditLoanDate"></div>
        <div class="fg"><label>ผู้ยืม *</label><input type="text" id="admEditBorrower" placeholder="ชื่อ-นามสกุล"></div>
        <div class="fg"><label>แผนก *</label><input type="text" id="admEditDept" placeholder="ชื่อแผนก"></div>
        <div class="fg"><label>รหัสแผนก</label><input type="text" id="admEditDeptCode" placeholder="0001"></div>
        <div class="fg"><label>ผู้ป่วย *</label><input type="text" id="admEditPatient" placeholder="ชื่อ-นามสกุลผู้ป่วย"></div>
        <div class="fg"><label>สถานะ</label>
          <select id="admEditStatus">
            <option value="PENDING">PENDING</option>
            <option value="ACTIVE">ACTIVE</option>
            <option value="RETURNED">RETURNED</option>
            <option value="CANCELLED">CANCELLED</option>
          </select>
        </div>
      </div>
      <div class="fg" style="margin-top:10px"><label>รายการเครื่องมือ (JSON)</label>
        <textarea id="admEditEqList" rows="4" style="font-family:var(--mo);font-size:12px" placeholder='[{"name":"Ventilator","qty":1,"detail":""}]'></textarea>
        <div style="font-size:10px;color:var(--mu);margin-top:3px">⚠️ แก้ไข JSON โดยตรง — ต้องเป็น JSON ที่ถูกต้องเท่านั้น</div>
      </div>
    </div>
    <div class="mftr">
      <button class="btn ber sm" onclick="admDeleteLoan(document.getElementById('admEditLoanID').value)">🗑 ลบใบยืมนี้</button>
      <button class="btn bg" onclick="closeM('mAdmEditLoan')">ยกเลิก</button>
      <button class="btn badm" onclick="admSaveEditLoan()">💾 บันทึกการแก้ไข</button>
    </div>
  </div>
</div>

<!-- MODAL: ADMIN EDIT BORROW -->
<div class="mbk" id="mAdmEditBorrow">
  <div class="mdl" style="max-width:650px">
    <div class="mhd"><div class="mttl" style="color:#f59e0b">✏️ Admin: แก้ไขบันทึกการยืม</div><button class="mcls" onclick="closeM('mAdmEditBorrow')">✕</button></div>
    <div class="mbdy">
      <input type="hidden" id="admEditRecID">
      <div style="background:rgba(245,158,11,.07);border:1px solid rgba(245,158,11,.25);border-radius:6px;padding:8px 12px;margin-bottom:12px;font-size:12px;color:var(--mu)">
        ⚠️ การแก้ไขจะบันทึกลง Supabase ทันที
      </div>
      <div class="fgrid">
        <div class="fg"><label>RecordID (ไม่สามารถแก้ไข)</label><input type="text" id="admEditRecIDDisp" disabled style="opacity:.5"></div>
        <div class="fg"><label>LoanID</label><input type="text" id="admEditRecLoanID" placeholder="LN-..."></div>
        <div class="fg"><label>ผู้ให้ยืม</label><input type="text" id="admEditGiver" placeholder="ชื่อผู้ให้ยืม"></div>
        <div class="fg"><label>หมายเลขเครื่อง</label><input type="text" id="admEditEqNo" placeholder="หมายเลขเครื่อง"></div>
        <div class="fg"><label>วันที่คืน</label><input type="date" id="admEditRetDate"></div>
        <div class="fg"><label>ผู้รับคืน</label><input type="text" id="admEditReceiver" placeholder="ชื่อผู้รับคืน"></div>
        <div class="fg"><label>เหตุผลคืน</label><input type="text" id="admEditRetRemark" placeholder="เหตุผล"></div>
        <div class="fg"><label>สถานะ</label>
          <select id="admEditRecStatus">
            <option value="ACTIVE">ACTIVE</option>
            <option value="RETURNED">RETURNED</option>
            <option value="ACC_PENDING">ACC_PENDING</option>
          </select>
        </div>
        <div class="fg">
          <label>คืนตัวเครื่องแล้ว?</label>
          <select id="admEditMachineRet">
            <option value="false">ยังไม่คืน (false)</option>
            <option value="true">คืนแล้ว (true)</option>
          </select>
        </div>
      </div>
    </div>
    <div class="mftr">
      <button class="btn ber sm" onclick="admDeleteBorrow(document.getElementById('admEditRecID').value)">🗑 ลบรายการนี้</button>
      <button class="btn bg" onclick="closeM('mAdmEditBorrow')">ยกเลิก</button>
      <button class="btn badm" onclick="admSaveEditBorrow()">💾 บันทึกการแก้ไข</button>
    </div>
  </div>
</div>

<!-- MODAL: ADD LOAN -->
<div class="mbk" id="mAddLoan">
  <div class="mdl mdsm">
    <div class="mhd"><div class="mttl" style="color:#f59e0b">➕ Admin: เพิ่มใบยืมใหม่</div><button class="mcls" onclick="closeM('mAddLoan')">✕</button></div>
    <div class="mbdy">
      <div class="fgrid">
        <div class="fg"><label>วันที่ยืม *</label><input type="date" id="addLoanDate"></div>
        <div class="fg"><label>ผู้ยืม *</label><input type="text" id="addLoanBorrower" placeholder="ชื่อ-นามสกุล"></div>
        <div class="fg"><label>แผนก *</label><input type="text" id="addLoanDept" placeholder="ชื่อแผนก"></div>
        <div class="fg"><label>รหัสแผนก</label><input type="text" id="addLoanDeptCode" placeholder="0001" value="9999"></div>
        <div class="fg"><label>ผู้ป่วย *</label><input type="text" id="addLoanPatient" placeholder="ชื่อ-นามสกุลผู้ป่วย"></div>
        <div class="fg"><label>เครื่องมือ *</label>
          <select id="addLoanEq">
            <!-- filled by JS -->
          </select>
        </div>
        <div class="fg"><label>รายละเอียด</label><input type="text" id="addLoanDetail" placeholder="รายละเอียดเพิ่มเติม"></div>
        <div class="fg"><label>สถานะเริ่มต้น</label>
          <select id="addLoanStatus">
            <option value="PENDING">PENDING</option>
            <option value="ACTIVE">ACTIVE</option>
          </select>
        </div>
      </div>
    </div>
    <div class="mftr">
      <button class="btn bg" onclick="closeM('mAddLoan')">ยกเลิก</button>
      <button class="btn badm" onclick="admAddLoan()">➕ เพิ่มใบยืม</button>
    </div>
  </div>
</div>

<script>
// ============================================================
//  ★★★  CONFIG  ★★★
// ============================================================
var SUPABASE_URL      = 'https://lkudggtehmgwcqntntts.supabase.co/rest/v1/';
var SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImxrdWRnZ3RlaG1nd2NxbnRudHRzIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzU5MDczOTUsImV4cCI6MjA5MTQ4MzM5NX0.vsUzZTzscF-AKlk-pKm86Hc5dfuFOx-loXmuLHsJBQY';
var APP_PIN           = '4321';
var ADMIN_PIN         = '9999';   // ← เปลี่ยน Admin PIN ได้ที่นี่
// ============================================================

SUPABASE_URL = SUPABASE_URL.replace(/\/rest\/v1\/?$/, '').replace(/\/$/, '');

var SB = {
  headers: function() {
    return {'Content-Type':'application/json','apikey':SUPABASE_ANON_KEY,'Authorization':'Bearer '+SUPABASE_ANON_KEY,'Prefer':'return=representation'};
  },
  url: function(table,qs){return SUPABASE_URL+'/rest/v1/'+table+(qs?'?'+qs:'');},
  get: async function(table,qs){var r=await fetch(SB.url(table,qs),{headers:SB.headers()});if(!r.ok)throw new Error(await r.text());return r.json();},
  post: async function(table,body){var r=await fetch(SB.url(table),{method:'POST',headers:SB.headers(),body:JSON.stringify(body)});if(!r.ok)throw new Error(await r.text());return r.json();},
  patch: async function(table,qs,body){var r=await fetch(SB.url(table,qs),{method:'PATCH',headers:SB.headers(),body:JSON.stringify(body)});if(!r.ok)throw new Error(await r.text());return r.json();},
  del: async function(table,qs){var r=await fetch(SB.url(table,qs),{method:'DELETE',headers:SB.headers()});if(!r.ok)throw new Error(await r.text());return r.status===204?[]:r.json();},
  rpc: async function(fn,body){var r=await fetch(SUPABASE_URL+'/rest/v1/rpc/'+fn,{method:'POST',headers:SB.headers(),body:JSON.stringify(body)});if(!r.ok)throw new Error(await r.text());return r.json();}
};

var API = {
  getAllLoans: async function(fullHistory){
    var cutoff=new Date();cutoff.setFullYear(cutoff.getFullYear()-1);var cutoffStr=cutoff.toISOString().split('T')[0];
    var query=fullHistory
      ?'order=created_at.desc&limit=50000'
      :'or=(status.eq.ACTIVE,status.eq.PENDING,status.eq.ACC_PENDING,and(status.eq.RETURNED,loan_date.gte.'+cutoffStr+'),and(status.eq.CANCELLED,loan_date.gte.'+cutoffStr+'))&order=created_at.desc&limit=10000';
    var rows=await SB.get('loan_requests',query);
    return rows.map(function(r){return{LoanID:r.loan_id,LoanDate:r.loan_date,BorrowerName:r.borrower_name,DeptCode:r.dept_code,DeptName:r.dept_name,PatientName:r.patient_name,EquipmentList:Array.isArray(r.equipment_list)?r.equipment_list:[],Status:r.status};});
  },
  getAllBorrows: async function(fullHistory){
    var cutoff=new Date();cutoff.setFullYear(cutoff.getFullYear()-1);var cutoffStr=cutoff.toISOString().split('T')[0];
    var query=fullHistory
      ?'order=created_at.desc&limit=100000'
      :'or=(status.eq.ACTIVE,status.eq.ACC_PENDING,and(status.eq.RETURNED,created_at.gte.'+cutoffStr+'T00:00:00))&order=created_at.desc&limit=20000';
    var rows=await SB.get('borrow_records',query);
    return rows.map(function(r){
      var ACC_MAP=[{id:'a1',name:'Oxygen flow meter pipeline with power take off',col:'acc1_flow_meter_pto'},{id:'a2',name:'Oxygen flow meter pipeline 70 LPM',col:'acc2_flow_meter_70'},{id:'a3',name:'Self inflating bag',col:'acc3_self_inflating'},{id:'a4',name:'full face mask',col:'acc4_full_face_mask'},{id:'a5',name:'Nasal cannula HFNC',col:'acc5_nasal_cannula'},{id:'a6',name:'อื่นๆ',col:'acc6_other'}];
      var accs=[];
      ACC_MAP.forEach(function(a){var val=r[a.col]||'';if(val&&val.trim())accs.push({id:a.id,name:a.name,no:val==='✓'?'':val});});
      var retAccs=r.returned_accs;if(!Array.isArray(retAccs))retAccs=[];
      var th=r.transfer_history;if(!Array.isArray(th))th=[];
      return{RecordID:r.record_id,LoanID:r.loan_id,GiverName:r.giver_name||'',EquipmentNo:r.equipment_no||'',EquipItemIndex:r.equip_item_index||0,Accessories:accs,ReturnRemark:r.return_remark||'',ReturnDetail:r.return_detail||'',ReturnDate:r.return_date||'',MachineReturnDate:r.machine_return_date||'',MachineReturned:r.machine_returned||false,ReturnedAccs:retAccs,TransferHistory:th,Status:r.status,ReceiverName:r.receiver_name||''};
    });
  },
  submitLoan: async function(data){
    var ts=new Date();var id='LN-'+ts.getFullYear()+String(ts.getMonth()+1).padStart(2,'0')+String(ts.getDate()).padStart(2,'0')+'-'+Math.random().toString(36).substr(2,8).toUpperCase();
    var rows=await SB.post('loan_requests',{loan_id:id,loan_date:data.loanDate,borrower_name:data.borrowerName,dept_code:data.deptCode,dept_name:data.deptName,patient_name:data.patientName,equipment_list:data.equipmentList||[],status:'PENDING'});
    return rows[0].loan_id;
  },
  updateLoan: async function(loanID,data){
    var body={};
    if(data.deptCode!==undefined)body.dept_code=data.deptCode;
    if(data.deptName!==undefined)body.dept_name=data.deptName;
    if(data.status!==undefined)body.status=data.status;
    if(data.loanDate!==undefined)body.loan_date=data.loanDate;
    if(data.borrowerName!==undefined)body.borrower_name=data.borrowerName;
    if(data.patientName!==undefined)body.patient_name=data.patientName;
    if(data.equipmentList!==undefined)body.equipment_list=data.equipmentList;
    await SB.patch('loan_requests','loan_id=eq.'+encodeURIComponent(loanID),body);
  },
  cancelLoan: async function(loanID){await API.updateLoan(loanID,{status:'CANCELLED'});},
  deleteLoan: async function(loanID){
    await SB.del('borrow_records','loan_id=eq.'+encodeURIComponent(loanID));
    await SB.del('loan_requests','loan_id=eq.'+encodeURIComponent(loanID));
  },
  submitBorrow: async function(data){
    var loanID=data.loanID,idx=parseInt(data.equipItemIndex,10)||0,eqNo=data.equipmentNo||'';
    if(eqNo){
      var existing=await SB.get('borrow_records','equipment_no=eq.'+encodeURIComponent(eqNo)+'&status=eq.ACTIVE&limit=10');
      if(existing.length>0){
        var eqNameNew=(data.equipmentName||'').trim().toLowerCase();
        var blocked=false;var blockMsg='';
        for(var _i=0;_i<existing.length;_i++){
          var _rec=existing[_i];
          var _loanRows=await SB.get('loan_requests','loan_id=eq.'+encodeURIComponent(_rec.loan_id));
          if(_loanRows.length){
            var _eqList=_loanRows[0].equipment_list||[];
            var _recIdx=typeof _rec.equip_item_index==='number'?_rec.equip_item_index:0;
            var _existName=(_eqList[_recIdx]&&_eqList[_recIdx].name?_eqList[_recIdx].name:'').trim().toLowerCase();
            if(!eqNameNew||!_existName||eqNameNew===_existName){
              blocked=true;
              blockMsg='หมายเลข "'+eqNo+'" ('+(_eqList[_recIdx]?_eqList[_recIdx].name:'?')+')\nถูกใช้งานอยู่แล้ว — แผนก: '+(_loanRows[0].dept_name||'ไม่ระบุ');
              break;
            }
          }
        }
        if(blocked)return{success:false,duplicate:true,error:blockMsg};
      }
    }
    var dupItem=await SB.get('borrow_records','loan_id=eq.'+encodeURIComponent(loanID)+'&equip_item_index=eq.'+idx+'&status=eq.ACTIVE&limit=1');
    if(dupItem.length>0)return{success:false,duplicate:true,error:'item #'+idx+' ของ '+loanID+' ถูกบันทึกแล้ว'};
    var ACC_ID_TO_COL={a1:'acc1_flow_meter_pto',a2:'acc2_flow_meter_70',a3:'acc3_self_inflating',a4:'acc4_full_face_mask',a5:'acc5_nasal_cannula',a6:'acc6_other'};
    var row={loan_id:loanID,giver_name:data.giverName||'',equipment_no:eqNo,equip_item_index:idx,status:'ACTIVE'};
    (data.accessories||[]).forEach(function(a){var col=ACC_ID_TO_COL[a.id];if(col)row[col]=(a.no&&a.no.trim())?a.no:'✓';});
    var ts=new Date();row.record_id='BR-'+ts.getFullYear()+String(ts.getMonth()+1).padStart(2,'0')+String(ts.getDate()).padStart(2,'0')+'-'+Math.random().toString(36).substr(2,8).toUpperCase();
    var inserted=await SB.post('borrow_records',row);
    return{success:true,recordID:inserted[0].record_id};
  },
  returnEquipment: async function(recordID,returnData){
    var recs=await SB.get('borrow_records','record_id=eq.'+encodeURIComponent(recordID));
    if(!recs.length)throw new Error('Record not found: '+recordID);
    var rec=recs[0];var machineReturned=returnData.machineReturned;var newStatus=machineReturned?'RETURNED':'ACTIVE';
    var prev=Array.isArray(rec.returned_accs)?rec.returned_accs:[];var added=Array.isArray(returnData.newReturnedAccs)?returnData.newReturnedAccs:[];
    added.forEach(function(nr){if(!prev.some(function(p){return p.id===nr.id;}))prev.push({id:nr.id,name:nr.name||'',no:nr.no||'',date:nr.date||returnData.returnDate||''});});
    var patchBody={return_remark:returnData.returnRemark||'',return_detail:returnData.returnDetail||'',return_date:returnData.returnDate||null,machine_returned:machineReturned,returned_accs:prev,status:newStatus,receiver_name:returnData.receiverName||''};if(machineReturned&&!rec.machine_return_date)patchBody.machine_return_date=returnData.returnDate||null;await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(recordID),patchBody);
  },
  transferEquipment: async function(recordID,transferData){
    var recs=await SB.get('borrow_records','record_id=eq.'+encodeURIComponent(recordID));
    if(!recs.length)throw new Error('Record not found: '+recordID);
    var th=Array.isArray(recs[0].transfer_history)?recs[0].transfer_history:[];
    th.push({date:transferData.date,fromDept:transferData.fromDept||'',toDept:transferData.toDept,transfererName:transferData.transfererName||'',note:transferData.note||''});
    await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(recordID),{transfer_history:th});
    var loanRows=await SB.get('borrow_records','record_id=eq.'+encodeURIComponent(recordID)+'&select=loan_id');
    if(loanRows.length)await API.updateLoan(loanRows[0].loan_id,{deptCode:transferData.toDeptCode,deptName:transferData.toDept});
  }
};

// ── DEFAULT DATA (overridden by localStorage if saved) ──────
var _DEPTS_DEFAULT=[{c:'0001',n:'ER'},{c:'0002',n:'100/9'},{c:'0003',n:'100/6'},{c:'0004',n:'80/10'},{c:'0005',n:'80/9'},{c:'0006',n:'80/8'},{c:'0007',n:'ศช'},{c:'0008',n:'ศญ'},{c:'0009',n:'80/5'},{c:'0010',n:'80/2'},{c:'0011',n:'PICU'},{c:'0012',n:'80/1'},{c:'0013',n:'14/9.'},{c:'0014',n:'14/8.'},{c:'0015',n:'14/7.'},{c:'0016',n:'อญ'},{c:'0017',n:'อช'},{c:'0018',n:'CICU'},{c:'0019',n:'SICU'},{c:'0020',n:'MICU'},{c:'0021',n:'OPD MED'},{c:'0022',n:'OPD SURG'},{c:'0023',n:'แผนกไตเทียม'},{c:'0024',n:'วิสัญญี'},{c:'0025',n:'OR'},{c:'0026',n:'ห้องตรวจพิเศษศัลย์'},{c:'0027',n:'MIS'},{c:'0028',n:'กายภาพ'},{c:'0029',n:'ห้อง Intervention'},{c:'0030',n:'รังสี'},{c:'0031',n:'OPD chest'},{c:'0032',n:'OPD ortho'},{c:'0033',n:'OPD กุมาร'},{c:'0034',n:'OPD ตา'},{c:'0035',n:'OPD ENT'},{c:'0036',n:'OPD Uro'},{c:'0037',n:'ศูนย์สมองฯ'},{c:'0038',n:'ศูนย์แพทย์'},{c:'9999',n:'อื่นๆ'}];
var _EQL_DEFAULT=['Ventilator','Ventilator transfer','HFNC','HFNC transfer','BiPAP','BIRD','Infusion pump','Infusion pump for chemo','Syringe pump','PCA','Enteral feeding pump','Oxygen flow meter pipeline','Oxygen flow meter regulator','Continuous suction pipeline','Intermittent suction pipeline','thoracic suction pipeline','PEEP VALVE','CPAP VALVE','Heated probe humidifier','อื่นๆ'];
var _EQNUMS_DEFAULT=['O2FLO1','O2FLO2','O2FLO3','MEK1','MEK2','MEK3','MEK5'];
var _RETREMARKS_DEFAULT=['OFF','ย้ายไปหน่วยวิกฤต','DEAD','เครื่องมืออุปกรณ์ชำรุดขัดข้อง','ไม่ได้ใช้งาน','NA','อื่นๆ'];
var _ACCS_DEFAULT=[{id:'a1',n:'Oxygen flow meter pipeline with power take off',no:true},{id:'a2',n:'Oxygen flow meter pipeline 70 LPM',no:true},{id:'a3',n:'Self inflating bag',no:false},{id:'a4',n:'full face mask',det:true},{id:'a5',n:'Nasal cannula HFNC',no:true},{id:'a6',n:'อื่นๆ',det:true}];
var _STAFF_DEFAULT=['ร.อ.วรชาติ','ร.อ. ประสิทธิ์','น.ต.วิทยา','พ.จ.อ.หญิง วนิดา','น.ต.หญิง นวิยา','น.ท.หญิง สุชาดา','น.ส.นิภาวรรณ','น.ส. ประนอม','อื่นๆ'];

function _lsGet(key,def){try{var v=localStorage.getItem('meq_'+key);return v?JSON.parse(v):def;}catch(e){return def;}}
function _lsSet(key,val){try{localStorage.setItem('meq_'+key,JSON.stringify(val));}catch(e){}}

var DEPTS=_lsGet('depts',_DEPTS_DEFAULT);
var EQL=_lsGet('eql',_EQL_DEFAULT);
var ACCS=_lsGet('accs',_ACCS_DEFAULT);
var STAFF=_lsGet('staff',_STAFF_DEFAULT);
var EQNUMS=_lsGet('eqnums',_EQNUMS_DEFAULT);
var RETREMARKS=_lsGet('retremarks',_RETREMARKS_DEFAULT);

// Working copies for list manager (edited before applying)
var _wDepts=null,_wStaff=null,_wEql=null,_wAcc=null;
var _selDeptIdx=-1,_selStaffIdx=-1,_selEqlIdx2=-1,_selAccIdx=-1;

var loans=null,borrows=null,selAccs={},_rMap={},_selEqIdx=-1,_borrowEqName='';
var _retBorrowData={accs:[],retAccs:[],recID:'',eqName:'',eqNo:'',machineAlreadyReturned:false};
var _pinUnlocked=false,_adminUnlocked=false,arTimer=null;
var _bulkPendingLoans=[],_bulkPendingBors=[];

document.addEventListener('DOMContentLoaded',function(){
  if(SUPABASE_URL.includes('YOUR_PROJECT_ID')||SUPABASE_ANON_KEY.includes('YOUR_ANON_KEY')){document.getElementById('cfgBanner').classList.add('on');}
  setToday();buildEqCards();buildAccTable();rebuildEqNoDatalist();rebuildRetRemarkSelect();
  buildSSL('deptList',DEPTS,function(c,n){pickDept('deptList',c,n);});
  buildSSL('trDeptList',DEPTS,function(c,n){pickDept('trDeptList',c,n);});
  buildStaff();initFilters();loadAll();
  // fill addLoan equipment select
  var sel=document.getElementById('addLoanEq');
  EQL.forEach(function(e){sel.innerHTML+='<option value="'+e+'">'+e+'</option>';});
  // fill admBorEq
  var sel2=document.getElementById('admBorEq');
  EQL.forEach(function(e){sel2.innerHTML+='<option value="'+e+'">'+e+'</option>';});
});

function setToday(){var t=new Date().toISOString().split('T')[0];['loanDate','retDate','trDate','addLoanDate','qdNewDate','qrNewRetDate'].forEach(function(id){var e=document.getElementById(id);if(e)e.value=t;});}
function buildStaff(){['giverSel','receiverSel','transfererSel'].forEach(function(id){var el=document.getElementById(id);if(!el)return;STAFF.forEach(function(s){var o=document.createElement('option');o.value=s;o.textContent=s;el.appendChild(o);});});}
function handleStaff(p){var sel=document.getElementById(p+'Sel'),oth=document.getElementById(p+'Oth');if(!sel||!oth)return;oth.style.display=sel.value==='อื่นๆ'?'block':'none';if(sel.value!=='อื่นๆ')oth.value='';}
function getStaff(p){var sel=document.getElementById(p+'Sel'),oth=document.getElementById(p+'Oth');if(!sel)return'';return sel.value==='อื่นๆ'?(oth?oth.value.trim():''):sel.value;}
function initFilters(){['allDept','recDept','dailyDept'].forEach(function(id){var el=document.getElementById(id);if(!el)return;DEPTS.forEach(function(d){el.innerHTML+='<option value="'+d.n+'">'+d.n+'</option>';});});['allEq','recEq','dailyFilter'].forEach(function(id){var el=document.getElementById(id);if(!el)return;EQL.forEach(function(e){el.innerHTML+='<option value="'+e+'">'+e+'</option>';});});}
function clearDailyFilter(){['dailySearch','dailyDateS','dailyDateE'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});['dailyFilter','dailyDept'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});renderDaily();}
function clearSumFilter(){['sumS','sumE'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});['sumDept','sumEq','sumStat'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});updateSumFilterBadges();loadSum();}

// ── Realtime refresh หน้าสถิติ ─────────────────────────────────
window._sumRealtimeTimer=null;
function startSumRealtime(){
  stopSumRealtime();
  window._sumRealtimeTimer=setInterval(function(){
    var pg=document.getElementById('pg-sum');
    if(pg&&pg.classList.contains('on')){
      renderSum({}); // async — fire and forget OK for timer
    }
  },60000); // ทุก 1 นาที
}
function stopSumRealtime(){
  if(window._sumRealtimeTimer){clearInterval(window._sumRealtimeTimer);window._sumRealtimeTimer=null;}
}
function updateSumFilterBadges(){
  var dept=(document.getElementById('sumDept')||{}).value||'';
  var eq=(document.getElementById('sumEq')||{}).value||'';
  var stat=(document.getElementById('sumStat')||{}).value||'';
  var ds=(document.getElementById('sumS')||{}).value||'';
  var de=(document.getElementById('sumE')||{}).value||'';
  var bd=document.getElementById('sumFilterBadges');if(!bd)return;
  var badges=[];
  if(ds||de)badges.push('<span style="background:rgba(0,200,255,.1);border:1px solid rgba(0,200,255,.3);border-radius:12px;padding:2px 10px;font-size:11px;color:var(--ac)">📅 '+(ds||'...')+' → '+(de||'...')+'</span>');
  if(dept)badges.push('<span style="background:rgba(16,185,129,.1);border:1px solid rgba(16,185,129,.3);border-radius:12px;padding:2px 10px;font-size:11px;color:var(--ok)">🏥 '+dept+'</span>');
  if(eq)badges.push('<span style="background:rgba(124,58,237,.1);border:1px solid rgba(124,58,237,.3);border-radius:12px;padding:2px 10px;font-size:11px;color:var(--ac2)">🔧 '+eq+'</span>');
  if(stat)badges.push('<span style="background:rgba(245,158,11,.1);border:1px solid rgba(245,158,11,.3);border-radius:12px;padding:2px 10px;font-size:11px;color:var(--wn)">● '+({ACTIVE:'ยืม',RETURNED:'คืน',ACC_PENDING:'อุปกรณ์ค้าง'}[stat]||stat)+'</span>');
  bd.style.display=badges.length?'flex':'none';
  bd.innerHTML=badges.length?badges.join(''):'';
}

function initSumFilters(){
  var ds=document.getElementById('sumDept');
  var es=document.getElementById('sumEq');
  if(ds&&ds.options.length<=1)DEPTS.forEach(function(d){ds.innerHTML+='<option value="'+d.n+'">'+d.n+'</option>';});
  if(es&&es.options.length<=1)EQL.forEach(function(e){es.innerHTML+='<option value="'+e+'">'+e+'</option>';});
  // section override selects
  var secSelects={
    'secBarEq':'eq','secDayEq':'eq',
    'secAvgEq':'eq',
    'secOvDept':'dept','secOvEq':'eq',
    'secAllDept':'dept','secAllEq':'eq'
  };
  Object.keys(secSelects).forEach(function(id){
    var el=document.getElementById(id);if(!el||el.options.length>1)return;
    if(secSelects[id]==='eq'){EQL.forEach(function(e){el.innerHTML+='<option value="'+e+'">'+e+'</option>';});}
    else{DEPTS.forEach(function(d){el.innerHTML+='<option value="'+d.n+'">'+d.n+'</option>';});}
  });
}

// ════════════════════════════════════════════════════════════
// STANDBY CARD FUNCTIONS
// ════════════════════════════════════════════════════════════
var _standbySelected=false;

function toggleStandby(){
  _standbySelected=!_standbySelected;
  var sc=document.getElementById('standbyCard');
  var sv=document.getElementById('standbyCheck');
  var sp=document.getElementById('standbyPanel');
  if(_standbySelected){
    // deselect regular cards first
    _selEqIdx=-1;
    EQL.forEach(function(_,j){var card=document.getElementById('eqcard_'+j),chk=document.getElementById('eqcheck_'+j);if(card){card.style.borderColor='var(--bd)';card.style.background='var(--bg)';card.style.boxShadow='';}if(chk)chk.style.display='none';});
    if(sc){sc.style.borderColor='#f59e0b';sc.style.background='rgba(245,158,11,.14)';sc.style.boxShadow='0 0 0 2px rgba(245,158,11,.4)';}
    if(sv)sv.style.display='flex';
    if(sp)sp.style.display='block';
    // focus eq no
    setTimeout(function(){var el=document.getElementById('sbyEqNo');if(el)el.focus();},100);
  } else {
    clearStandby();
  }
}

function clearStandby(){
  _standbySelected=false;
  var sc=document.getElementById('standbyCard');
  var sv=document.getElementById('standbyCheck');
  var sp=document.getElementById('standbyPanel');
  if(sc){sc.style.borderColor='var(--wn)';sc.style.background='rgba(245,158,11,.05)';sc.style.boxShadow='';}
  if(sv)sv.style.display='none';
  if(sp)sp.style.display='none';
  // reset fields
  ['sbyEqNo','sbyFmPtoNo','sbyFm70No'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});
  ['sbyFmPto','sbyFm70'].forEach(function(id){var e=document.getElementById(id);if(e)e.checked=false;});
  document.querySelectorAll('input[name="standbyType"]').forEach(function(r){r.checked=false;});
  var fs=document.getElementById('sbyFlowSection');if(fs)fs.style.display='none';
  var sm=document.getElementById('sbySummary');if(sm)sm.style.display='none';
  updateStandbyLabels();
}

function onStandbyTypeChange(){
  var fs=document.getElementById('sbyFlowSection');
  var sbyType=document.querySelector('input[name="standbyType"]:checked');
  if(fs)fs.style.display=sbyType?'block':'none';
  updateStandbyLabels();
  updateStandbySummary();
}

function updateStandbyLabels(){
  // highlight selected radio label
  ['sbyVentLabel','sbyHfncLabel'].forEach(function(lid){
    var el=document.getElementById(lid);if(!el)return;
    var radio=el.querySelector('input[type=radio]');
    if(radio&&radio.checked){el.style.borderColor='var(--wn)';el.style.background='rgba(245,158,11,.12)';el.style.color='var(--wn)';}
    else{el.style.borderColor='var(--bd)';el.style.background='var(--bg)';el.style.color='var(--tx)';}
  });
}

function updateStandbySummary(){
  var sm=document.getElementById('sbySummary');if(!sm)return;
  var sbyType=document.querySelector('input[name="standbyType"]:checked');
  var sbyNo=document.getElementById('sbyEqNo').value.trim();
  if(!sbyType&&!sbyNo){sm.style.display='none';return;}
  var parts=[];
  if(sbyType)parts.push('⏸ '+sbyType.value+' Standby');
  if(sbyNo)parts.push('No. '+sbyNo);
  var fmPto=document.getElementById('sbyFmPto');var fmPtoNo=document.getElementById('sbyFmPtoNo').value.trim();
  var fm70=document.getElementById('sbyFm70');var fm70No=document.getElementById('sbyFm70No').value.trim();
  if(fmPto&&fmPto.checked)parts.push('Flow meter PTO'+(fmPtoNo?' #'+fmPtoNo:''));
  if(fm70&&fm70.checked)parts.push('Flow meter 70LPM'+(fm70No?' #'+fm70No:''));
  sm.style.display='block';
  sm.innerHTML='📋 สรุป: '+parts.join(' | ');
}

// live update summary on any input change inside standby panel
document.addEventListener('DOMContentLoaded',function(){
  var panel=document.getElementById('standbyPanel');
  if(panel){
    panel.addEventListener('input',function(){updateStandbySummary();});
    panel.addEventListener('change',function(){updateStandbySummary();updateStandbyLabels();});
  }
});

function buildEqCards(){var grid=document.getElementById('eqCardGrid');if(!grid)return;EQL.forEach(function(name,i){var card=document.createElement('div');card.id='eqcard_'+i;card.style.cssText='border:2px solid var(--bd);border-radius:8px;padding:12px 14px;cursor:pointer;transition:all .18s;background:var(--bg);position:relative;min-height:54px;display:flex;flex-direction:column;justify-content:center;gap:6px';card.innerHTML='<div style="font-size:13px;font-weight:600;color:var(--tx);line-height:1.4;padding-right:22px">'+name+'</div>'+(name==='อื่นๆ'?'<input type="text" id="oth_'+i+'" placeholder="ระบุรายละเอียด..." onclick="event.stopPropagation()" style="font-size:12px;padding:5px 8px;width:100%;border:1px solid var(--bd);border-radius:4px;background:var(--s1);color:var(--tx)">':'')
      +(['Ventilator','HFNC'].indexOf(name)>=0?
        '<div id="vhpanel_'+i+'" onclick="event.stopPropagation()" style="display:none;margin-top:10px;background:rgba(0,200,255,.05);border:1px solid rgba(0,200,255,.25);border-radius:7px;padding:12px">'
          +'<div style="font-size:10px;font-weight:700;color:var(--ac);text-transform:uppercase;letter-spacing:.5px;margin-bottom:9px">🔩 รายละเอียดเพิ่มเติมลงเฉพาะเครื่อง standby</div>'
          +'<div style="display:grid;grid-template-columns:1fr 1fr;gap:10px">'
            +'<div>'
              +'<label style="font-size:11px;color:var(--mu);font-weight:700;display:block;margin-bottom:4px">หมายเลขเครื่อง (No.)</label>'
              +'<input type="text" id="vheqno_'+i+'" placeholder="เช่น V-05, 12 ..." onclick="event.stopPropagation()" style="width:100%;padding:6px 8px;font-size:13px;background:var(--bg);border:1px solid rgba(0,200,255,.4);border-radius:5px;color:var(--tx)">'
            +'</div>'
            +'<div>'
              +'<label style="font-size:11px;color:var(--mu);font-weight:700;display:block;margin-bottom:4px">Flow Meter No.</label>'
              +'<input type="text" id="vhfmno_'+i+'" placeholder="เช่น 15, FM-03 ..." onclick="event.stopPropagation()" style="width:100%;padding:6px 8px;font-size:13px;background:var(--bg);border:1px solid rgba(0,200,255,.4);border-radius:5px;color:var(--tx)">'
            +'</div>'
          +'</div>'
        +'</div>'
      :'')
      +'<div id="eqcheck_'+i+'" style="display:none;position:absolute;top:8px;right:8px;width:22px;height:22px;background:var(--ac);border-radius:50%;align-items:center;justify-content:center;font-size:12px;color:#000;font-weight:700">✓</div>';card.addEventListener('click',function(){selEq(i);});grid.appendChild(card);});}
function selEq(i){
  // deselect standby
  _standbySelected=false;
  var sc=document.getElementById('standbyCard');var sv=document.getElementById('standbyCheck');var sp=document.getElementById('standbyPanel');
  if(sc){sc.style.borderColor='var(--wn)';sc.style.background='rgba(245,158,11,.05)';}
  if(sv)sv.style.display='none';if(sp)sp.style.display='none';
  // select this card — hide all vhpanels first
  EQL.forEach(function(_,j){var card=document.getElementById('eqcard_'+j),chk=document.getElementById('eqcheck_'+j);if(card){card.style.borderColor='var(--bd)';card.style.background='var(--bg)';card.style.boxShadow='';}if(chk)chk.style.display='none';var vhp=document.getElementById('vhpanel_'+j);if(vhp)vhp.style.display='none';});
  var card=document.getElementById('eqcard_'+i),chk=document.getElementById('eqcheck_'+i);
  if(card){card.style.borderColor='var(--ac)';card.style.background='rgba(0,200,255,.06)';card.style.boxShadow='0 0 0 1px var(--ac)';}
  if(chk)chk.style.display='flex';
  // show VH panel for Ventilator / HFNC only
  var vhp=document.getElementById('vhpanel_'+i);
  if(vhp){vhp.style.display='block';setTimeout(function(){var el=document.getElementById('vheqno_'+i);if(el)el.focus();},80);}
  _selEqIdx=i;
}
function buildAccTable(){document.getElementById('accBody').innerHTML=ACCS.map(function(a){var inp;if(a.no){inp='<div style="display:flex;align-items:center;gap:4px"><span style="font-size:12px;color:var(--mu);font-weight:600;white-space:nowrap">No.</span><input type="number" id="accno_'+a.id+'" placeholder="0" min="0" style="width:80px;padding:4px 6px;font-size:13px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)"></div>';}else if(a.det){inp='<input type="text" id="accno_'+a.id+'" placeholder="'+(a.id==='a4'?'ระบุหมายเลข/รายละเอียด...':'รายละเอียด...')+'" style="width:160px;padding:4px 6px;font-size:12px;background:var(--bg);border:1px solid var(--bd);border-radius:4px;color:var(--tx)">';}else{inp='-';}return'<tr><td style="padding:5px 7px;border-bottom:1px solid var(--bd)"><input type="checkbox" id="acc_'+a.id+'" onchange="toggleAcc(\''+a.id+'\',\''+a.n.replace(/'/g,"\\'")+'\')"></td><td style="padding:5px 7px;border-bottom:1px solid var(--bd);font-size:13px">'+a.n+'</td><td style="padding:5px 7px;border-bottom:1px solid var(--bd)">'+inp+'</td></tr>';}).join('');}
function toggleAcc(id,name){var c=document.getElementById('acc_'+id);if(c.checked){var n=document.getElementById('accno_'+id);selAccs[id]={id:id,name:name,no:n?String(n.value).trim():''};}else delete selAccs[id];renderAccTags();}
function renderAccTags(){document.getElementById('accTags').innerHTML=Object.values(selAccs).map(function(a){var accDef=ACCS.find(function(x){return x.id===a.id;});var valTxt='';if(a.no){if(accDef&&accDef.no)valTxt=' <span style="font-family:var(--mo);color:var(--ac);font-weight:700">No.'+a.no+'</span>';else valTxt=' <span style="color:var(--mu);font-size:11px">('+a.no+')</span>';}return'<span style="display:inline-flex;align-items:center;gap:3px;background:rgba(124,58,237,.15);border:1px solid rgba(124,58,237,.3);border-radius:11px;padding:2px 8px;font-size:11px;margin:2px">'+a.name+valTxt+' <span style="cursor:pointer;color:var(--er);font-weight:700" onclick="rmAcc(\''+a.id+'\')">✕</span></span>';}).join('');}
function rmAcc(id){delete selAccs[id];var e=document.getElementById('acc_'+id);if(e)e.checked=false;renderAccTags();}

function buildSSL(listId,items,onSel){var el=document.getElementById(listId);if(!el)return;el.innerHTML=items.map(function(d){return'<div class="ssi" data-c="'+d.c+'" data-n="'+d.n+'"><span>'+d.n+'</span></div>';}).join('');el.addEventListener('click',function(e){var it=e.target.closest('.ssi');if(it)onSel(it.dataset.c,it.dataset.n);});}
function filterSSL(id,q){var ql=q.toLowerCase();document.getElementById(id).querySelectorAll('.ssi').forEach(function(el){el.style.display=el.textContent.toLowerCase().includes(ql)?'':'none';});}
function openSSL(id){document.getElementById(id).classList.add('on');}
document.addEventListener('click',function(e){if(!e.target.closest('.ssw'))document.querySelectorAll('.ssl').forEach(function(l){l.classList.remove('on');});});
function pickDept(lid,code,name){if(lid==='deptList'){document.getElementById('selDeptCode').value=code;document.getElementById('selDeptName').value=name;document.getElementById('deptSearch').value=name;var d=document.getElementById('selDeptDisp');d.style.display='block';var oi=document.getElementById('deptOther');if(name==='อื่นๆ'){d.innerHTML='✅ <strong>อื่นๆ</strong> — <span style="color:var(--wn);font-size:11px">กรุณาระบุชื่อแผนกด้านล่าง</span>';if(oi){oi.style.display='block';oi.focus();}}else{d.innerHTML='✅ <strong>'+name+'</strong>';if(oi){oi.style.display='none';oi.value='';}}}else{document.getElementById('trDeptCode').value=code;document.getElementById('trDeptName').value=name;document.getElementById('trDeptSearch').value=name;var d2=document.getElementById('trDeptDisp');d2.style.display='block';d2.innerHTML='✅ <strong>'+name+'</strong>';}document.getElementById(lid).classList.remove('on');}
function updateDeptOther(){var oi=document.getElementById('deptOther');var code=document.getElementById('selDeptCode').value;if(code==='9999'&&oi){var val=oi.value.trim();document.getElementById('selDeptName').value=val||'อื่นๆ';var d=document.getElementById('selDeptDisp');if(d)d.innerHTML='✅ <strong>'+(val||'อื่นๆ')+'</strong>';}}

async function loadAll(){
  var rb=document.getElementById('recBody');if(rb)rb.innerHTML='<tr><td colspan="5"><div class="lding"><div class="spin"></div> กำลังโหลด...</div></td></tr>';
  var dc=document.getElementById('dailyCont');if(dc)dc.innerHTML='<div class="lding"><div class="spin"></div> กำลังโหลด...</div>';
  try{
    var[ld,bd]=await Promise.all([API.getAllLoans(),API.getAllBorrows()]);
    loans=ld;borrows=bd;
    // auto-sync loan statuses that mismatch borrow_records
    (function(){var exp=expanded();var lsm={};exp.forEach(function(item){var lid=S(item.loan.LoanID);if(!lsm[lid])lsm[lid]=[];lsm[lid].push(item.status);});var toFix=[];for(var lid in lsm){var st=lsm[lid];var hasPend=st.indexOf('PENDING')>=0,hasAct=st.indexOf('ACTIVE')>=0,hasAccP=st.indexOf('ACC_PENDING')>=0;var allRet=st.every(function(s){return s==='RETURNED'||s==='ACC_PENDING';});var allCanc=st.every(function(s){return s==='CANCELLED';});var ns=allCanc?'CANCELLED':hasPend?'PENDING':hasAct?'ACTIVE':hasAccP?'ACC_PENDING':allRet?'RETURNED':'PENDING';var cur=loans.find(function(l){return l.LoanID===lid;});if(cur&&S(cur.Status)!==ns&&S(cur.Status)!=='CANCELLED'){toFix.push({lid:lid,ns:ns,cur:cur});}}if(toFix.length){Promise.all(toFix.map(function(x){return API.updateLoan(x.lid,{status:x.ns}).then(function(){x.cur.Status=x.ns;}).catch(function(){});})).then(function(){renderRec();renderAll();renderDaily();updateDash();});}})();
    renderRec();renderAll();renderDaily();updateDash();
    // ถ้าหน้าสถิติเปิดอยู่ตอน loadAll เสร็จ → load ทันที
    if(document.getElementById('pg-sum')&&document.getElementById('pg-sum').classList.contains('on')){
      loadSum().then(function(){startSumRealtime();});
    }
    if(document.getElementById('adm-loan-mgr')&&document.getElementById('adm-loan-mgr').classList.contains('on'))renderAdminLoans();
    if(document.getElementById('adm-borrow-mgr')&&document.getElementById('adm-borrow-mgr').classList.contains('on'))renderAdminBorrows();
    if(arTimer)clearTimeout(arTimer);
    arTimer=setTimeout(loadAll,15*60*1000); // auto-refresh ทุก 15 นาที
  }catch(e){console.error(e);toast('โหลดข้อมูลล้มเหลว: '+e.message,'error');if(rb)rb.innerHTML='<tr><td colspan="5" style="text-align:center;color:var(--er);padding:24px">❌ โหลดข้อมูลล้มเหลว</td></tr>';}
}

function S(v){return String(v===null||v===undefined?'':v).trim();}
function N(v){var n=parseInt(String(v===null||v===undefined?'0':v),10);return isNaN(n)?0:n;}
function computeStatus(borrow){if(!borrow)return'PENDING';var raw=S(borrow.Status).toUpperCase();var accs=Array.isArray(borrow.Accessories)?borrow.Accessories:[];var retAccs=Array.isArray(borrow.ReturnedAccs)?borrow.ReturnedAccs:[];var machineReturned=borrow.MachineReturned===true||raw==='RETURNED';if(!machineReturned)return'ACTIVE';if(accs.length===0)return'RETURNED';var pending=accs.filter(function(a){return!retAccs.some(function(r){return r.id===a.id;});});return pending.length>0?'ACC_PENDING':'RETURNED';}
function expanded(){if(!loans||!borrows)return[];var out=[];loans.forEach(function(loan){if(loan.Status==='CANCELLED'){(loan.EquipmentList||[]).forEach(function(eq,idx){out.push({loan:loan,eq:eq,idx:idx,borrow:null,status:'CANCELLED'});});return;}(loan.EquipmentList||[]).forEach(function(eq,idx){var bw=borrows.find(function(b){return S(b.LoanID)===S(loan.LoanID)&&N(b.EquipItemIndex)===idx;});out.push({loan:loan,eq:eq,idx:idx,borrow:bw||null,status:computeStatus(bw)});});});return out;}
var STATUS_ORDER={PENDING:0,ACTIVE:1,ACC_PENDING:2,RETURNED:3,CANCELLED:4};
function sortItems(items){return items.slice().sort(function(a,b){var pa=STATUS_ORDER[a.status]!==undefined?STATUS_ORDER[a.status]:5;var pb=STATUS_ORDER[b.status]!==undefined?STATUS_ORDER[b.status]:5;if(pa!==pb)return pa-pb;return(b.loan.LoanDate||'').localeCompare(a.loan.LoanDate||'');});}
function badge(s){var m={PENDING:['bp2','รอดำเนินการจ่าย'],ACTIVE:['ba','ยืม'],RETURNED:['br2','คืน'],ACC_PENDING:['bap','คืน (อุปกรณ์ค้าง)'],CANCELLED:['bc2','ยกเลิก']};var p=m[s]||['bp2',s];return'<span class="badge '+p[0]+'">'+p[1]+'</span>';}
function standbyBadge(eq){if(!eq)return'';var n=S(eq.name||'');if(!n.toLowerCase().includes('standby'))return'';return' <span style="display:inline-flex;align-items:center;gap:3px;background:rgba(245,158,11,.15);border:1px solid rgba(245,158,11,.4);border-radius:10px;padding:1px 7px;font-size:10px;font-weight:700;color:#f59e0b;vertical-align:middle">⏸ STANDBY</span>';}
function buildTransferTrail(loan,borrow){if(!borrow)return'';var th=Array.isArray(borrow.TransferHistory)?borrow.TransferHistory:[];if(!th.length)return'';var crumbs=[];if(th[0]&&S(th[0].fromDept)){crumbs.push(S(th[0].fromDept));}else{var _m=th[0]&&S(th[0].note).match(/โอนจาก\s*(.+)/);crumbs.push(_m&&_m[1].trim()?_m[1].trim():S(loan.DeptName));}th.forEach(function(t){crumbs.push(S(t.toDept));});var chips=crumbs.map(function(dept,i){var isLast=i===crumbs.length-1;if(isLast)return'<span class="tr-chip-cur">'+dept+' <span class="tr-now">ปัจจุบัน</span></span>';return'<span class="tr-chip">'+dept+'</span> <span class="tr-arr">›</span> ';}).join('');var last=th[th.length-1];var meta=last?'<div class="tr-meta">โอนล่าสุด '+S(last.date)+(last.transfererName?' · '+S(last.transfererName):'')+' </div>':'';return'<div class="transfer-trail"><span class="tr-label">จากแผนก</span> '+chips+meta+'</div>';}

function renderRec(){
  var tbody=document.getElementById('recBody');if(!tbody)return;
  if(loans===null){tbody.innerHTML='<tr><td colspan="5"><div class="lding"><div class="spin"></div></div></td></tr>';return;}
  var search=document.getElementById('recSearch').value.toLowerCase();var sf=document.getElementById('recStat').value;var ds=document.getElementById('recDateS').value,de=document.getElementById('recDateE').value;var feq=document.getElementById('recEq')?document.getElementById('recEq').value:'';var fdept=document.getElementById('recDept')?document.getElementById('recDept').value:'';
  var items=expanded().filter(function(item){if(sf==='TRANSFERRED'){if(!(item.borrow&&Array.isArray(item.borrow.TransferHistory)&&item.borrow.TransferHistory.length>0))return false;}else if(sf&&item.status!==sf)return false;if(feq&&S(item.eq.name)!==feq)return false;if(fdept&&S(item.loan.DeptName)!==fdept)return false;var ld=item.loan.LoanDate;if(ds&&ld&&ld<ds)return false;if(de&&ld&&ld>de)return false;var txt=S(item.loan.LoanID)+' '+S(item.loan.BorrowerName)+' '+S(item.loan.DeptName)+' '+S(item.loan.PatientName)+' '+S(item.eq.name)+' '+(item.borrow?S(item.borrow.EquipmentNo):'');return!search||txt.toLowerCase().includes(search);});
  if(!items.length){tbody.innerHTML='<tr><td colspan="5" style="text-align:center;color:var(--mu);padding:24px">ไม่มีข้อมูล</td></tr>';return;}
  _rMap={};var groups=[],gmap={};items.forEach(function(item){var lid=S(item.loan.LoanID);if(!gmap[lid]){gmap[lid]=[];groups.push({lid:lid,loan:item.loan,items:gmap[lid]});}gmap[lid].push(item);});
  groups.sort(function(a,b){var pa=Math.min.apply(null,a.items.map(function(i){return STATUS_ORDER[i.status]!==undefined?STATUS_ORDER[i.status]:5;}));var pb=Math.min.apply(null,b.items.map(function(i){return STATUS_ORDER[i.status]!==undefined?STATUS_ORDER[i.status]:5;}));if(pa!==pb)return pa-pb;return(b.loan.LoanDate||'').localeCompare(a.loan.LoanDate||'');});
  var rowN=0;
  var html=groups.map(function(g){
    rowN++;var loan=g.loan,ss=g.items.map(function(i){return i.status;});var hasPending=ss.indexOf('PENDING')>=0,hasActive=ss.indexOf('ACTIVE')>=0;var allRet=ss.every(function(s){return s==='RETURNED'||s==='ACC_PENDING';});var hasAccP=ss.indexOf('ACC_PENDING')>=0;var grpStat=hasPending?'PENDING':hasActive?'ACTIVE':hasAccP?'ACC_PENDING':allRet?'RETURNED':'CANCELLED';
    var cancelBtn='';
    if(loan.Status!=='CANCELLED'&&loan.Status!=='RETURNED'&&hasPending&&!hasActive)cancelBtn='<button class="abt" style="color:var(--er);border-color:var(--er);font-size:11px" data-act="cancelAll" data-lid="'+S(loan.LoanID)+'">✕ ยกเลิก</button>';
    if(loan.Status==='CANCELLED')cancelBtn='<button class="abt" style="color:var(--er);border-color:var(--er);font-size:11px" data-act="deleteAll" data-lid="'+S(loan.LoanID)+'">🗑 ลบ</button>';
    var hdr='<tr class="loan-hdr"><td style="font-family:var(--mo);color:var(--mu);font-size:10px;font-weight:700">'+rowN+'</td><td colspan="3" style="padding:8px 10px"><div style="display:flex;flex-wrap:wrap;gap:10px;align-items:center"><span style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+S(loan.LoanID)+'</span><span>📅 '+loan.LoanDate+'</span><span>ผู้ยืม: <strong>'+S(loan.BorrowerName)+'</strong></span><span>แผนก: <strong>'+S(loan.DeptName)+'</strong></span><span>ผู้ป่วย: '+S(loan.PatientName)+'</span>'+badge(grpStat)+'<span style="font-size:10px;color:var(--mu)">'+g.items.length+' รายการ</span></div></td><td style="text-align:right;white-space:nowrap">'+cancelBtn+'</td></tr>';
    var subRows=sortItems(g.items).map(function(item){var key=S(item.loan.LoanID)+'__'+item.idx;_rMap[key]=item;var eqNo=item.borrow?S(item.borrow.EquipmentNo)||'—':'—';var trailHtml=item.borrow&&item.borrow.TransferHistory&&item.borrow.TransferHistory.length?buildTransferTrail(item.loan,item.borrow):'';var btns='<button class="abt" data-act="view" data-key="'+key+'">👁</button>';if(item.status==='PENDING')btns+=' <button class="abt" style="border-color:var(--ac);color:var(--ac)" data-act="borrow" data-key="'+key+'">📝 บันทึก</button>';if(item.status==='ACTIVE')btns+=' <button class="abt" style="border-color:var(--ok);color:var(--ok)" data-act="return" data-key="'+key+'">↩ คืน</button>';if(item.status==='ACTIVE')btns+=' <button class="abt" style="border-color:var(--wn);color:var(--wn)" data-act="transfer" data-key="'+key+'">🔄 โอน</button>';if(item.status==='ACC_PENDING')btns+=' <button class="abt" style="border-color:#f59e0b;color:#f59e0b" data-act="retacc" data-key="'+key+'">📦 คืนอุปกรณ์</button>';if(item.status==='CANCELLED')btns+=' <button class="abt" style="border-color:var(--er);color:var(--er)" data-act="delete" data-key="'+key+'">🗑 ลบ</button>';return'<tr class="item-row"><td style="text-align:center;font-family:var(--mo);color:var(--mu);font-size:10px">└ '+item.idx+'</td><td><span style="background:var(--s2);border-radius:3px;padding:1px 6px;font-family:var(--mo);font-size:10px;color:var(--ac2)">item '+item.idx+'</span> <strong>'+S(item.eq.name)+'</strong>'+standbyBadge(item.eq)+(item.eq.detail?'  <span style="font-size:11px;color:var(--mu)">'+item.eq.detail+'</span>':'')+trailHtml+'</td><td style="font-family:var(--mo);font-size:13px;color:var(--ac)">'+eqNo+'</td><td>'+badge(item.status)+(item.borrow&&item.borrow.TransferHistory&&item.borrow.TransferHistory.length?'<span class="badge btr" style="margin-left:3px;font-size:9px">🔄 โอน '+item.borrow.TransferHistory.length+'x</span>':'')+'</td><td><div style="display:flex;gap:3px;flex-wrap:nowrap">'+btns+'</div></td></tr>';}).join('');
    return hdr+subRows;
  }).join('');
  tbody.innerHTML=html;
  if(tbody._h)tbody.removeEventListener('click',tbody._h);
  tbody._h=function(e){var btn=e.target.closest('[data-act]');if(!btn)return;var act=btn.dataset.act,key=btn.dataset.key,lid=btn.dataset.lid;if(act==='cancelAll'&&lid){requirePin(function(){cancelLoan(lid);});return;}if(act==='deleteAll'&&lid){deleteLoan(lid);return;}var item=_rMap[key];if(!item)return;if(act==='view')viewDet(S(item.loan.LoanID));else if(act==='borrow'){var _i=item;requirePin(function(){openBorrow(S(_i.loan.LoanID),_i.idx);});}else if(act==='return'){var _i2=item;requirePin(function(){openReturn(_i2.borrow?S(_i2.borrow.RecordID):'',S(_i2.loan.LoanID),_i2.idx);});}else if(act==='retacc'){var _i3=item;requirePin(function(){openReturnAcc(_i3.borrow?S(_i3.borrow.RecordID):'',S(_i3.loan.LoanID),_i3.idx);});}else if(act==='transfer'){var _i4=item;requirePin(function(){openTransfer(_i4.borrow?S(_i4.borrow.RecordID):'',S(_i4.loan.DeptName),S(_i4.loan.LoanID),_i4.idx);});}else if(act==='delete')deleteLoan(S(item.loan.LoanID));};
  tbody.addEventListener('click',tbody._h);
}

function requirePin(onSuccess){if(_pinUnlocked){onSuccess();return;}var overlay=document.createElement('div');overlay.style.cssText='position:fixed;inset:0;background:rgba(0,0,0,.75);z-index:8000;display:flex;align-items:center;justify-content:center;padding:16px';overlay.innerHTML='<div style="background:var(--s1);border:2px solid var(--ac);border-radius:12px;padding:32px 36px;text-align:center;max-width:360px;width:100%"><div style="font-size:40px;margin-bottom:10px">🔐</div><div style="font-size:20px;font-weight:700;color:var(--ac);margin-bottom:6px">ใส่รหัสผ่าน</div><div style="font-size:13px;color:var(--mu);margin-bottom:18px">สำหรับ จนท. แผนกบริการเครื่องมือแพทย์ฯ เท่านั้น</div><input type="password" id="pinInput" placeholder="รหัสผ่าน" maxlength="10" style="width:100%;padding:12px;font-size:18px;text-align:center;letter-spacing:6px;background:var(--bg);border:1px solid var(--bd);border-radius:7px;color:var(--tx);margin-bottom:14px"><div id="pinErr" style="color:var(--er);font-size:13px;margin-bottom:10px;min-height:18px"></div><div style="display:flex;gap:8px"><button style="flex:1;padding:11px;background:transparent;border:1px solid var(--bd);border-radius:7px;color:var(--tx);font-size:14px;font-weight:600;cursor:pointer;font-family:var(--fn)" onclick="this.closest(\'[data-pin]\').remove()">ยกเลิก</button><button style="flex:1;padding:11px;background:var(--ac);border:none;border-radius:7px;color:#000;font-size:14px;font-weight:700;cursor:pointer;font-family:var(--fn)" onclick="checkPin(this)">ยืนยัน</button></div></div>';overlay.setAttribute('data-pin','1');overlay.addEventListener('click',function(e){if(e.target===overlay)overlay.remove();});document.body.appendChild(overlay);setTimeout(function(){var inp=document.getElementById('pinInput');if(inp)inp.focus();},100);overlay._cb=onSuccess;var inp=document.getElementById('pinInput');if(inp)inp.addEventListener('keydown',function(e){if(e.key==='Enter'){var btn=overlay.querySelector('button:last-child');if(btn)btn.click();}});}
function checkPin(btn){var overlay=btn.closest('[data-pin]');var inp=document.getElementById('pinInput');if(!inp||!overlay)return;if(inp.value===APP_PIN){_pinUnlocked=true;overlay.remove();if(overlay._cb)overlay._cb();}else{var err=document.getElementById('pinErr');if(err)err.textContent='รหัสไม่ถูกต้อง';inp.value='';inp.focus();setTimeout(function(){if(err)err.textContent='';},2000);}}

async function submitLoan(){
  var loanDate=document.getElementById('loanDate').value;
  var borrowerName=document.getElementById('borrowerName').value.trim();
  var deptCode=document.getElementById('selDeptCode').value;
  var deptName=document.getElementById('selDeptName').value;
  var patientName=document.getElementById('patientName').value.trim();
  if(!loanDate||!borrowerName||!deptCode||!patientName){bigToast('กรุณากรอกข้อมูลให้ครบถ้วน','error');return;}
  if(deptCode==='9999'){var oi=document.getElementById('deptOther');var ov=oi?oi.value.trim():'';if(!ov){bigToast('กรุณาระบุชื่อแผนก/หน่วยงาน\nในช่อง "อื่นๆ"','error');if(oi)oi.focus();return;}deptName=ov;}
  var equipmentList=[];
  if(_standbySelected){
    // ── STANDBY path ──
    var sbyType=document.querySelector('input[name="standbyType"]:checked');
    var sbyNo=document.getElementById('sbyEqNo').value.trim();
    if(!sbyType){bigToast('กรุณาเลือกประเภทเครื่อง Standby\n(Ventilator หรือ HFNC)','error');return;}
    if(!sbyNo){bigToast('กรุณากรอกหมายเลขเครื่อง Standby','error');return;}
    var detail='Standby | No.'+sbyNo;
    var flowParts=[];
    var fmPto=document.getElementById('sbyFmPto');var fmPtoNo=document.getElementById('sbyFmPtoNo').value.trim();
    var fm70=document.getElementById('sbyFm70');var fm70No=document.getElementById('sbyFm70No').value.trim();
    if(fmPto&&fmPto.checked){flowParts.push('Flow meter PTO'+(fmPtoNo?' No.'+fmPtoNo:''));}
    if(fm70&&fm70.checked){flowParts.push('Flow meter 70LPM'+(fm70No?' No.'+fm70No:''));}
    if(flowParts.length)detail+=' | '+flowParts.join(', ');
    equipmentList=[{name:sbyType.value+' standby',qty:1,detail:detail,standby:true,eqNo:sbyNo}];
  } else {
    // ── regular eq path ──
    if(_selEqIdx<0){bigToast('กรุณาเลือกรายการเครื่องมือ 1 รายการ','error');return;}
    var name=EQL[_selEqIdx];var oEl=document.getElementById('oth_'+_selEqIdx);
    // ── Ventilator / HFNC extra fields (ไม่บังคับ) ──
    var _vhEqNo='',_vhFmNo='';
    if(['Ventilator','HFNC'].indexOf(name)>=0){
      var _vhEqNoEl=document.getElementById('vheqno_'+_selEqIdx);
      var _vhFmNoEl=document.getElementById('vhfmno_'+_selEqIdx);
      _vhEqNo=_vhEqNoEl?_vhEqNoEl.value.trim():'';
      _vhFmNo=_vhFmNoEl?_vhFmNoEl.value.trim():'';
    }
    var _vhParts=[];
    if(_vhEqNo)_vhParts.push('No.'+_vhEqNo);
    if(_vhFmNo)_vhParts.push('Flow Meter No.'+_vhFmNo);
    var _baseDetail=oEl?oEl.value.trim():'';
    var _fullDetail=_vhParts.length?(_vhParts.join(' | ')+(_baseDetail?' | '+_baseDetail:'')):_baseDetail;
    equipmentList=[{name:name,qty:1,detail:_fullDetail}];
  }
  try{
    var id=await API.submitLoan({loanDate,borrowerName,deptCode,deptName,patientName,equipmentList});
    bigToast('ส่งแบบฟอร์มสำเร็จ!\nID: '+id,'success');clearLoan();setTimeout(loadAll,1500);
  }catch(e){bigToast('เกิดข้อผิดพลาด:\n'+e.message,'error');}
}
function clearLoan(){
  ['borrowerName','patientName','deptSearch','selDeptCode','selDeptName'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});
  document.getElementById('selDeptDisp').style.display='none';
  var oi=document.getElementById('deptOther');if(oi){oi.style.display='none';oi.value='';}
  // clear regular eq cards
  _selEqIdx=-1;
  EQL.forEach(function(_,i){var card=document.getElementById('eqcard_'+i),chk=document.getElementById('eqcheck_'+i);if(card){card.style.borderColor='var(--bd)';card.style.background='var(--bg)';card.style.boxShadow='';}if(chk)chk.style.display='none';var o=document.getElementById('oth_'+i);if(o)o.value='';var vhp=document.getElementById('vhpanel_'+i);if(vhp)vhp.style.display='none';var vhn=document.getElementById('vheqno_'+i);if(vhn)vhn.value='';var vhf=document.getElementById('vhfmno_'+i);if(vhf)vhf.value='';});
  // clear standby
  clearStandby();
  setToday();
}
async function cancelLoan(lid){if(!confirm('ยืนยันการยกเลิกใบยืม '+lid+' ?'))return;try{await API.cancelLoan(lid);bigToast('ยกเลิกใบยืมแล้ว\n'+lid,'warning');setTimeout(loadAll,1500);}catch(e){bigToast('เกิดข้อผิดพลาด:\n'+e.message,'error');}}
async function deleteLoan(lid){if(!confirm('ลบรายการ '+lid+' ออกจากระบบ? (ไม่สามารถกู้คืนได้)'))return;try{await API.deleteLoan(lid);bigToast('ลบรายการแล้ว\n'+lid,'success');setTimeout(loadAll,1500);}catch(e){bigToast('เกิดข้อผิดพลาด:\n'+e.message,'error');}}

async function autoRecalcLoan(loanID){if(!loanID||!loans||!borrows)return;var exp=expanded();var items=exp.filter(function(item){return S(item.loan.LoanID)===S(loanID);});if(!items.length)return;var statuses=items.map(function(i){return i.status;});var hasPending=statuses.indexOf('PENDING')>=0;var hasActive=statuses.indexOf('ACTIVE')>=0;var hasAccP=statuses.indexOf('ACC_PENDING')>=0;var allRet=statuses.every(function(s){return s==='RETURNED'||s==='ACC_PENDING';});var allCancelled=statuses.every(function(s){return s==='CANCELLED';});var newStatus=allCancelled?'CANCELLED':hasPending?'PENDING':hasActive?'ACTIVE':hasAccP?'ACC_PENDING':allRet?'RETURNED':'PENDING';var current=loans.find(function(l){return S(l.LoanID)===S(loanID);});if(current&&S(current.Status)!==newStatus){try{await API.updateLoan(loanID,{status:newStatus});}catch(e){console.warn('autoRecalcLoan error:',e);}}}
function openBorrow(loanID,idx){var loan=loans.find(function(l){return S(l.LoanID)===S(loanID);});var eq=loan?(loan.EquipmentList||[])[idx]:null;_borrowEqName=eq?S(eq.name):'';document.getElementById('bLoanID').value=S(loanID);document.getElementById('bIdx').value=idx;document.getElementById('giverSel').value='';document.getElementById('giverOth').style.display='none';document.getElementById('giverOth').value='';document.getElementById('eqNo').value='';selAccs={};buildAccTable();renderAccTags();document.getElementById('bInfo').innerHTML=loan?'<div style="display:grid;grid-template-columns:1fr 1fr;gap:5px"><div>📅 '+loan.LoanDate+'</div><div>ผู้ยืม: <strong>'+loan.BorrowerName+'</strong></div><div>แผนก: <strong>'+loan.DeptName+'</strong></div><div>ผู้ป่วย: '+loan.PatientName+'</div></div>':'';document.getElementById('bItem').innerHTML=eq?'🔧 <span style="color:var(--ac)">'+eq.name+'</span>'+(eq.detail?' — '+eq.detail:''):'-';openM('mBorrow');}
async function saveBorrow(){var loanID=S(document.getElementById('bLoanID').value);var idx=N(document.getElementById('bIdx').value);var giver=getStaff('giver');var eqNo=S(document.getElementById('eqNo').value);if(!loanID){bigToast('ไม่พบ LoanID','error');return;}if(!giver){bigToast('กรุณาเลือกหรือระบุผู้ให้ยืม','error');return;}Object.keys(selAccs).forEach(function(id){var n=document.getElementById('accno_'+id);if(n)selAccs[id].no=String(n.value).trim();});var btn=document.getElementById('saveBBtn');if(btn){btn.disabled=true;btn.innerHTML='⏳ กำลังบันทึก...';}try{var r=await API.submitBorrow({loanID,equipItemIndex:idx,giverName:giver,equipmentNo:eqNo,equipmentName:_borrowEqName,accessories:Object.values(selAccs)});if(btn){btn.disabled=false;btn.innerHTML='💾 บันทึก';}if(r&&r.duplicate){bigToast(r.error,'warning');return;}bigToast('บันทึกสำเร็จ!\n'+r.recordID,'success');closeM('mBorrow');setTimeout(async function(){await loadAll();await autoRecalcLoan(loanID);},800);}catch(e){if(btn){btn.disabled=false;btn.innerHTML='💾 บันทึก';}bigToast('บันทึกล้มเหลว:\n'+e.message,'error');}}

function openReturn(recID,loanID,idx){if(!recID){bigToast('ไม่พบข้อมูลการยืม\nกรุณาบันทึกการยืมก่อน','error');return;}var loan=loans.find(function(l){return S(l.LoanID)===S(loanID);});var bw=borrows.find(function(b){return S(b.RecordID)===S(recID);});if(!bw){bigToast('ไม่พบข้อมูลการยืม (RecordID: '+recID+')','error');return;}var eq=loan?(loan.EquipmentList||[])[idx]:null;_retBorrowData.accs=bw.Accessories||[];_retBorrowData.retAccs=bw.ReturnedAccs||[];_retBorrowData.recID=recID;_retBorrowData.loanID=S(loanID);_retBorrowData.eqName=eq?S(eq.name):'?';_retBorrowData.eqNo=S(bw.EquipmentNo||'—');_retBorrowData.machineAlreadyReturned=bw.MachineReturned===true;document.getElementById('retRecID').value=recID;document.getElementById('receiverSel').value='';document.getElementById('receiverOth').style.display='none';document.getElementById('retRemark').value='';document.getElementById('retDet').value='';document.getElementById('retDetGrp').style.display='none';document.getElementById('retDate').value=new Date().toISOString().split('T')[0];var trailSection=bw.TransferHistory&&bw.TransferHistory.length&&loan?'<div style="margin-top:6px">'+buildTransferTrail(loan,bw)+'</div>':'';document.getElementById('retInfo').innerHTML='<strong>'+_retBorrowData.eqName+'</strong> เลขเครื่อง: <span style="font-family:var(--mo);color:var(--ac)">'+_retBorrowData.eqNo+'</span><br>แผนก: <strong>'+(loan?S(loan.DeptName):'-')+'</strong> | ผู้ป่วย: '+(loan?S(loan.PatientName):'-')+' | ผู้ให้ยืม: '+S(bw.GiverName||'-')+trailSection;var machineCb=document.getElementById('retMachineCb');var machineStatus=document.getElementById('retMachineStatus');document.getElementById('retMachineLbl').textContent=_retBorrowData.eqName+' (เลขเครื่อง: '+_retBorrowData.eqNo+')';if(_retBorrowData.machineAlreadyReturned){machineCb.checked=true;machineCb.disabled=true;machineCb.style.opacity='0.5';machineStatus.textContent='✓ คืนแล้ว';machineStatus.style.color='var(--ok)';}else{machineCb.checked=false;machineCb.disabled=false;machineCb.style.opacity='1';machineStatus.textContent='⏳ ยังไม่คืน';machineStatus.style.color='var(--er)';}var accs=_retBorrowData.accs,retAccs=_retBorrowData.retAccs;var retAccSection=document.getElementById('retAccSection'),retAccList=document.getElementById('retAccList');if(accs.length>0){retAccSection.style.display='block';var rhtml='';accs.forEach(function(a){var isRet=retAccs.some(function(r){return r.id===a.id;});var nameHtml=(a.name||'?')+(a.no?' <span style="font-family:var(--mo);color:var(--ac);font-size:11px">No.'+a.no+'</span>':'');if(isRet){rhtml+='<div style="display:flex;align-items:center;gap:10px;padding:8px 4px;border-bottom:1px solid var(--bd);opacity:.6"><span style="width:20px;height:20px;border-radius:4px;background:var(--ok);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:13px">✓</span><span style="font-size:13px;text-decoration:line-through">'+nameHtml+'</span><span style="font-size:11px;color:var(--ok);margin-left:auto">คืนแล้ว</span></div>';}else{rhtml+='<div style="display:flex;align-items:center;gap:10px;padding:8px 4px;border-bottom:1px solid var(--bd)"><input type="checkbox" id="retcb_'+a.id+'" value="'+a.id+'" style="width:18px;height:18px;accent-color:var(--ac);cursor:pointer;flex-shrink:0" onchange="retUpdateStatus()"><label for="retcb_'+a.id+'" style="font-size:13px;cursor:pointer;flex:1">'+nameHtml+'</label><span style="font-size:11px;color:var(--er)">⏳ ยังไม่คืน</span></div>';}});retAccList.innerHTML=rhtml;}else{retAccSection.style.display='none';}retUpdateStatus();openM('mReturn');}
function retUpdateStatus(){var machineCb=document.getElementById('retMachineCb');var machineStatus=document.getElementById('retMachineStatus');var machineReturned=_retBorrowData.machineAlreadyReturned||(machineCb&&machineCb.checked);if(machineStatus&&!_retBorrowData.machineAlreadyReturned){machineStatus.textContent=machineCb&&machineCb.checked?'✓ คืนแล้ว':'⏳ ยังไม่คืน';machineStatus.style.color=machineCb&&machineCb.checked?'var(--ok)':'var(--er)';}var accs=_retBorrowData.accs,retAccs=_retBorrowData.retAccs;var alreadyCount=0;accs.forEach(function(a){if(retAccs.some(function(r){return r.id===a.id;}))alreadyCount++;});var newlyChecked=0;accs.forEach(function(a){var cb=document.getElementById('retcb_'+a.id);if(cb&&cb.checked)newlyChecked++;});var totalAccRet=alreadyCount+newlyChecked,accRem=accs.length-totalAccRet;var summary=document.getElementById('retAccSummary');if(summary&&accs.length>0)summary.textContent='อุปกรณ์คืนแล้ว '+totalAccRet+'/'+accs.length+' รายการ'+(accRem>0?' — ค้างอีก '+accRem+' รายการ':'');var statusBox=document.getElementById('retStatusBox');if(statusBox){if(!machineReturned&&newlyChecked===0)statusBox.innerHTML='<span style="color:var(--mu)">เลือกรายการที่ต้องการคืนในครั้งนี้</span>';else if(!machineReturned&&newlyChecked>0)statusBox.innerHTML='<span style="color:var(--wn)">⚠️ คืนเฉพาะอุปกรณ์ — ตัวเครื่องยังไม่คืน สถานะยังเป็น "ยืม"</span>';else if(accs.length===0||accRem===0)statusBox.innerHTML='<span style="color:var(--ok)">✅ คืนครบทั้งหมด — สถานะจะเปลี่ยนเป็น "คืน"</span>';else statusBox.innerHTML='<span style="color:var(--ok)">✅ คืนตัวเครื่องแล้ว — สถานะเปลี่ยนเป็น "คืน"<br><span style="color:var(--wn);font-size:11px">⚠️ อุปกรณ์ยังค้าง '+accRem+' รายการ</span></span>';}}
function showRetDet(){var v=document.getElementById('retRemark').value;document.getElementById('retDetGrp').style.display=(v==='ย้ายไปหน่วยวิกฤต'||v==='เครื่องมือชำรุด'||v==='อื่นๆ')?'block':'none';}
async function saveReturn(){var recID=S(document.getElementById('retRecID').value);var receiver=getStaff('receiver');var remark=document.getElementById('retRemark').value;var detail=document.getElementById('retDet').value.trim();var date=document.getElementById('retDate').value;if(!recID||!receiver||!remark||!date){bigToast('กรุณากรอกข้อมูลให้ครบถ้วน','error');return;}var machineCb=document.getElementById('retMachineCb');var machineReturned=_retBorrowData.machineAlreadyReturned||(machineCb&&machineCb.checked);var accs=_retBorrowData.accs,retAccs=_retBorrowData.retAccs;var newRetAccs=[];accs.forEach(function(a){var cb=document.getElementById('retcb_'+a.id);if(cb&&cb.checked)newRetAccs.push({id:a.id,name:a.name||'',no:a.no||'',date:date});});if(_retBorrowData.machineAlreadyReturned&&newRetAccs.length===0){bigToast('กรุณาเลือกอุปกรณ์ที่ต้องการคืน','error');return;}if(!machineReturned&&newRetAccs.length===0){bigToast('กรุณาเลือกรายการที่ต้องการคืน','error');return;}var totalAccAfter=retAccs.length+newRetAccs.length;var allAccRet=(accs.length===0)||(totalAccAfter>=accs.length);try{await API.returnEquipment(recID,{receiverName:receiver,returnRemark:remark,returnDetail:detail,returnDate:date,newReturnedAccs:newRetAccs,machineReturned,allReturned:machineReturned});var msg,typ;if(_retBorrowData.machineAlreadyReturned){msg=allAccRet?'คืนอุปกรณ์ครบแล้ว!':'บันทึกคืนอุปกรณ์ '+newRetAccs.length+' รายการ';typ='success';}else if(machineReturned){var accMsg=accs.length>0&&!allAccRet?'\nอุปกรณ์ยังค้าง '+(accs.length-totalAccAfter)+' รายการ':'';msg='คืนสำเร็จ! สถานะเปลี่ยนเป็น "คืน"'+accMsg;typ='success';}else{msg='บันทึกคืนอุปกรณ์ '+newRetAccs.length+' รายการ\nตัวเครื่องยังไม่คืน';typ='warning';}bigToast(msg,typ);closeM('mReturn');var _retLID=_retBorrowData.loanID||'';setTimeout(async function(){await loadAll();if(_retLID)await autoRecalcLoan(_retLID);},800);}catch(e){bigToast('บันทึกการคืนล้มเหลว\n'+e.message,'error');}}
function openReturnAcc(recID,loanID,idx){openReturn(recID,loanID,idx);var mttl=document.querySelector('#mReturn .mttl');if(mttl)mttl.textContent='📦 คืนอุปกรณ์ที่ค้าง';var ri=document.getElementById('retInfo');if(ri)ri.innerHTML+='<div style="margin-top:6px;padding:5px 8px;background:rgba(16,185,129,.08);border:1px solid rgba(16,185,129,.3);border-radius:4px;font-size:12px;color:var(--ok)">✓ ตัวเครื่องคืนแล้ว — กรุณาเลือกอุปกรณ์ที่ต้องการคืนเพิ่ม</div>';}
function openTransfer(recID,fromDept,loanID,idx){if(!recID){toast('ไม่พบข้อมูลการยืม','error');return;}document.getElementById('trRecID').value=recID;document.getElementById('transfererSel').value='';document.getElementById('transfererOth').style.display='none';['trDeptCode','trDeptName','trDeptSearch'].forEach(function(id){document.getElementById(id).value='';});document.getElementById('trDeptDisp').style.display='none';document.getElementById('trNote').value='โอนจาก '+fromDept;document.getElementById('trDate').value=new Date().toISOString().split('T')[0];var loan=loans.find(function(l){return S(l.LoanID)===S(loanID);});var bw=borrows?borrows.find(function(b){return S(b.RecordID)===S(recID);}):null;var trailSection=bw&&bw.TransferHistory&&bw.TransferHistory.length&&loan?'<div style="margin-top:7px">'+buildTransferTrail(loan,bw)+'</div>':'';var eq=loan?(loan.EquipmentList||[])[idx]:null;document.getElementById('trInfo').innerHTML=(eq?'🔧 <strong>'+eq.name+'</strong> | แผนกปัจจุบัน: <span style="color:var(--ac)">'+fromDept+'</span>':'')+trailSection;openM('mTransfer');}
async function saveTransfer(){var recID=S(document.getElementById('trRecID').value);var transferer=getStaff('transferer');var toCode=S(document.getElementById('trDeptCode').value);var toDept=S(document.getElementById('trDeptName').value);var date=document.getElementById('trDate').value;var note=document.getElementById('trNote').value;if(!transferer){toast('กรุณาเลือกผู้โอน','error');return;}if(!toCode){toast('กรุณาเลือกแผนกที่โอนไป','error');return;}try{var _fromDept=S(document.getElementById('trNote').value).replace(/^โอนจาก\s*/,'');await API.transferEquipment(recID,{date,fromDept:_fromDept,toDept,toDeptCode:toCode,note,transfererName:transferer});toast('โอนสำเร็จ!');closeM('mTransfer');setTimeout(loadAll,2000);}catch(e){toast('เกิดข้อผิดพลาด: '+e.message,'error');}}

function renderDaily(){
  var cont=document.getElementById('dailyCont');if(!cont)return;
  if(loans===null){cont.innerHTML='<div class="lding"><div class="spin"></div></div>';return;}
  var filter=document.getElementById('dailyFilter').value;var search=(document.getElementById('dailySearch')?document.getElementById('dailySearch').value:'').toLowerCase().trim();var deptF=document.getElementById('dailyDept')?document.getElementById('dailyDept').value:'';var dateS=document.getElementById('dailyDateS')?document.getElementById('dailyDateS').value:'';var dateE=document.getElementById('dailyDateE')?document.getElementById('dailyDateE').value:'';var ts=document.getElementById('dailyTs');if(ts)ts.textContent='อัปเดต: '+new Date().toLocaleTimeString('th-TH');
  var now=new Date();var active=expanded().filter(function(i){if(i.status!=='ACTIVE')return false;if(deptF&&S(i.loan.DeptName)!==deptF)return false;var ld=i.loan.LoanDate;if(dateS&&ld&&ld<dateS)return false;if(dateE&&ld&&ld>dateE)return false;if(search){var txt=(i.borrow?S(i.borrow.EquipmentNo):'')+' '+S(i.loan.PatientName)+' '+S(i.loan.BorrowerName)+' '+(i.borrow?S(i.borrow.GiverName||''):'')+' '+S(i.loan.DeptName)+' '+S(i.eq.name);if(!txt.toLowerCase().includes(search))return false;}return true;});
  var grp={};active.forEach(function(item){var k=item.eq.name;if(!grp[k])grp[k]=[];grp[k].push(item);});var keys=filter?(grp[filter]?[filter]:[]):EQL.filter(function(e){return grp[e];});var _dMap={};
  if(!keys.length){cont.innerHTML='<div style="text-align:center;padding:50px 20px"><div style="font-size:42px;margin-bottom:9px">✅</div><div style="font-size:14px;color:var(--mu)">ไม่มีรายการกำลังยืมในขณะนี้</div></div>';return;}
  var html=keys.map(function(eqName){var items=(grp[eqName]||[]).slice().sort(function(a,b){var an=parseInt((a.borrow?S(a.borrow.EquipmentNo):'').replace(/\D/g,''))||9999;var bn=parseInt((b.borrow?S(b.borrow.EquipmentNo):'').replace(/\D/g,''))||9999;return an-bn;});var rows=items.map(function(item){var ld=item.loan.LoanDate||'-';var days=ld&&ld!=='-'?Math.max(0,Math.round((now-new Date(ld))/86400000)):'-';var eqNo=item.borrow?S(item.borrow.EquipmentNo)||'—':'—';var dayC=typeof days==='number'&&days>30?'var(--er)':typeof days==='number'&&days>14?'var(--wn)':'var(--ok)';var accs=item.borrow&&Array.isArray(item.borrow.Accessories)?item.borrow.Accessories:[];var dk=S(item.loan.LoanID)+'__'+item.idx+'_d';_dMap[dk]=item;var trailCell='<span style="color:var(--mu)">—</span>';if(item.borrow&&item.borrow.TransferHistory&&item.borrow.TransferHistory.length){var lt=item.borrow.TransferHistory[item.borrow.TransferHistory.length-1];trailCell='<span class="badge btr" style="font-size:9px">🔄 '+item.borrow.TransferHistory.length+'x</span><div style="font-size:10px;color:var(--ac2);margin-top:2px">→ '+S(lt.toDept)+'</div>';}return'<tr><td style="padding:8px 11px"><span style="font-family:var(--mo);font-weight:700;font-size:14px;color:var(--ac)">'+eqNo+'</span></td><td style="padding:8px 11px">'+ld+'</td><td style="padding:8px 11px"><strong>'+S(item.loan.DeptName)+'</strong></td><td style="padding:8px 11px">'+S(item.loan.PatientName)+'</td><td style="padding:8px 11px;font-family:var(--mo);font-weight:700;color:'+dayC+';text-align:center">'+days+'</td><td style="padding:8px 11px">'+S(item.borrow?item.borrow.GiverName||'-':'-')+'</td><td style="padding:8px 11px">'+trailCell+'</td><td style="padding:8px 11px">'+(accs.length>0?'<span style="font-size:11px;color:var(--ac2);font-weight:600">'+accs.length+' รายการ</span>':'<span style="color:var(--mu)">-</span>')+'</td><td style="padding:8px 11px;text-align:center"><button class="abt dvb" data-key="'+dk+'" style="border-color:var(--ac);color:var(--ac);font-size:11px">รายละเอียด</button></td></tr>';}).join('');
  return'<div class="card" style="margin-bottom:14px;padding:0;overflow:hidden"><div style="background:linear-gradient(135deg,rgba(0,200,255,.12),rgba(124,58,237,.08));border-bottom:1px solid var(--bd);padding:10px 14px;display:flex;align-items:center;justify-content:space-between"><div style="font-size:15px;font-weight:700;color:var(--ac)">🔧 '+eqName+'</div><div style="font-family:var(--mo);font-size:20px;font-weight:700;color:var(--ac)">'+items.length+' <span style="font-size:12px;color:var(--mu)">เครื่อง</span></div></div><div style="overflow-x:auto"><table style="width:100%;border-collapse:collapse;font-size:13px"><thead><tr style="background:rgba(0,0,0,.2)"><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">หมายเลขเครื่อง</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">วันที่ยืม</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">แผนก</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">ชื่อผู้ป่วย</th><th style="padding:7px 11px;text-align:center;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">วันที่ใช้</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">ผู้ให้ยืม</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">ประวัติโอน</th><th style="padding:7px 11px;text-align:left;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">อุปกรณ์ประกอบ</th><th style="padding:7px 11px;width:80px;text-align:center;font-size:10px;color:var(--mu);border-bottom:1px solid var(--bd)">รายละเอียด</th></tr></thead><tbody>'+rows+'</tbody></table></div></div>';
  }).join('');
  cont.innerHTML=html;if(cont._dh)cont.removeEventListener('click',cont._dh);cont._dh=function(e){var btn=e.target.closest('.dvb');if(btn&&btn.dataset.key&&_dMap[btn.dataset.key])viewDet(S(_dMap[btn.dataset.key].loan.LoanID));};cont.addEventListener('click',cont._dh);
}

function renderAll(){
  var tbody=document.getElementById('allBody');if(!tbody)return;
  if(loans===null){tbody.innerHTML='<tr><td colspan="11"><div class="lding"><div class="spin"></div></div></td></tr>';return;}
  var search=document.getElementById('allSearch').value.toLowerCase();var dept=document.getElementById('allDept').value,eq=document.getElementById('allEq').value;var stat=document.getElementById('allStat').value;var ds=document.getElementById('allDateS').value,de=document.getElementById('allDateE').value;var now=new Date();var _aMap={};
  var items=sortItems(expanded().filter(function(item){if(stat&&item.status!==stat)return false;if(dept&&S(item.loan.DeptName)!==dept)return false;if(eq&&S(item.eq.name)!==eq)return false;var ld=item.loan.LoanDate;if(ds&&ld&&ld<ds)return false;if(de&&ld&&ld>de)return false;var txt=S(item.loan.LoanID)+' '+S(item.loan.BorrowerName)+' '+S(item.loan.DeptName)+' '+S(item.loan.PatientName)+' '+S(item.eq.name)+' '+(item.borrow?S(item.borrow.EquipmentNo):'');return!search||txt.toLowerCase().includes(search);}));
  if(!items.length){tbody.innerHTML='<tr><td colspan="11" style="text-align:center;color:var(--mu);padding:24px">ไม่มีข้อมูล</td></tr>';return;}
  var html=items.map(function(item){var ld=item.loan.LoanDate,startD=ld?new Date(ld):null,endD=item.borrow&&item.borrow.ReturnDate?new Date(item.borrow.ReturnDate):now;var days=startD&&(item.status==='ACTIVE'||item.status==='RETURNED'||item.status==='ACC_PENDING')?Math.max(0,Math.round((endD-startD)/86400000)):'-';var ak=S(item.loan.LoanID)+'__'+item.idx+'_a';_aMap[ak]=item;var accs=item.borrow&&Array.isArray(item.borrow.Accessories)?item.borrow.Accessories:[];return'<tr><td style="font-family:var(--mo);font-size:10px;color:var(--mu)">'+S(item.loan.LoanID)+'<br><span style="color:var(--ac)">#'+item.idx+'</span></td><td>'+ld+'</td><td>'+S(item.loan.BorrowerName)+'</td><td>'+S(item.loan.DeptName)+'</td><td>'+S(item.loan.PatientName)+'</td><td><strong>'+S(item.eq.name)+'</strong>'+(item.eq.detail?'<br><span style="font-size:11px;color:var(--mu)">'+item.eq.detail+'</span>':'')+'</td><td style="font-family:var(--mo);font-size:12px">'+(item.borrow?S(item.borrow.EquipmentNo)||'—':'—')+'</td><td>'+(accs.length>0?'<span style="font-size:11px;color:var(--ac2);font-weight:600">'+accs.length+' รายการ</span>':'<span style="color:var(--mu)">-</span>')+'</td><td style="font-family:var(--mo);text-align:center">'+days+'</td><td>'+badge(item.status)+(item.borrow&&item.borrow.TransferHistory&&item.borrow.TransferHistory.length?'<span class="badge btr" style="margin-left:3px;font-size:9px">🔄 โอน</span>':'')+'</td><td style="text-align:center"><button class="abt avb" data-key="'+ak+'" style="border-color:var(--ac);color:var(--ac);font-size:11px">รายละเอียด</button></td></tr>';}).join('');
  tbody.innerHTML=html;if(tbody._ah)tbody.removeEventListener('click',tbody._ah);tbody._ah=function(e){var btn=e.target.closest('.avb');if(btn&&btn.dataset.key&&_aMap[btn.dataset.key])viewDet(S(_aMap[btn.dataset.key].loan.LoanID));};tbody.addEventListener('click',tbody._ah);
}

function updateDash(){
  var cont=document.getElementById('dashCont');
  if(!loans){cont.innerHTML='<div class="lding"><div class="spin"></div> กำลังโหลด...</div>';return;}
  var exp=expanded();
  var now=new Date();
  var activeItems=exp.filter(function(i){return i.status==='ACTIVE';});
  var pendingCount=exp.filter(function(i){return i.status==='PENDING';}).length;
  var retCount=exp.filter(function(i){return i.status==='RETURNED';}).length;
  var accPendingCount=exp.filter(function(i){return i.status==='ACC_PENDING';}).length;
  var overMonth=activeItems.filter(function(i){return i.loan.LoanDate&&(now-new Date(i.loan.LoanDate))>30*86400000;}).length;

  // equipment usage bar
  var eqUsage={};
  activeItems.forEach(function(item){var n=S(item.eq.name);if(!eqUsage[n])eqUsage[n]=0;eqUsage[n]++;});
  var eqB=Object.entries(eqUsage).sort(function(a,b){return b[1]-a[1];}).slice(0,15);
  var mA=Math.max.apply(null,eqB.map(function(e){return e[1];}).concat([1]));

  // HFNC data
  var HFNC_EQL=['HFNC','HFNC transfer'];
  var HFNC_BUILDINGS={
    'อาคาร 14 ชั้น':['14/9.','14/8.','14/7.','อญ','อช'],
    'อาคารเชิดบุญเมือง':['MICU'],
    'อาคาร 100 ปี':['100/9','100/6'],
    'อาคาร 80 พรรษา':['80/10','80/9','80/8','ศช','ศญ','80/5','PICU','80/1']
  };
  var hfncItems=activeItems.filter(function(i){return HFNC_EQL.some(function(n){return S(i.eq.name)===n;});});
  var bC={},dC={};
  hfncItems.forEach(function(i){
    var dept=S(i.loan.DeptName);
    dC[dept]=(dC[dept]||0)+1;
    for(var b in HFNC_BUILDINGS){if(HFNC_BUILDINGS[b].indexOf(dept)>=0)bC[b]=(bC[b]||0)+1;}
  });
  var wbList=['อาคาร 14 ชั้น','อาคาร 100 ปี','อาคาร 80 พรรษา','อาคารเชิดบุญเมือง'];
  var hfncTotal=wbList.reduce(function(s,b){return s+(bC[b]||0);},0);
  var LIMIT_100=30,LIMIT_ALL=65;
  var b100=bC['อาคาร 100 ปี']||0;
  var warn100=b100>LIMIT_100,warnAll=hfncTotal>LIMIT_ALL;

  // dept table
  var deptMap={};
  activeItems.forEach(function(item){
    var dept=S(item.loan.DeptName)||'ไม่ระบุ';
    if(!deptMap[dept])deptMap[dept]={};
    var eq=S(item.eq.name);
    if(!deptMap[dept][eq])deptMap[dept][eq]=0;
    deptMap[dept][eq]++;
  });
  var allEqNames={};activeItems.forEach(function(item){allEqNames[S(item.eq.name)]=true;});
  var eqNames=Object.keys(allEqNames).sort();
  var deptList=Object.keys(deptMap).sort(function(a,b){
    var ta=Object.values(deptMap[a]).reduce(function(s,v){return s+v;},0);
    var tb=Object.values(deptMap[b]).reduce(function(s,v){return s+v;},0);
    return tb-ta;
  });

  // ── HFNC dept stats for summary cards ──
  var dE=Object.entries(dC).sort(function(a,b){return b[1]-a[1];});
  var maxDept=dE.length?dE[0]:null;
  var minDept=dE.length?dE[dE.length-1]:null;
  var mHD=Math.max.apply(null,dE.map(function(e){return e[1];}).concat([1]));
  var bE=Object.entries(bC);
  var mB=Math.max.apply(null,bE.map(function(e){return e[1];}).concat([1]));

  // ── render stats cards ──
  var warnHtml=warnAll?'<div class="hwarn"><span style="font-size:20px">⚠️</span><div><strong style="color:var(--er)">แจ้งเตือน HFNC</strong><br><span style="font-size:12px">ยอดรวม = <strong>'+hfncTotal+'</strong> เครื่อง — เกินเกณฑ์ 65!</span></div></div>':'';
  var statsHtml=warnHtml+'<div class="sgrid">'
    +'<div class="sc scy"><div class="snum" style="color:var(--ac)">'+activeItems.length+'</div><div class="slbl">กำลังยืม</div></div>'
    +'<div class="sc syw"><div class="snum" style="color:var(--wn)">'+pendingCount+'</div><div class="slbl">รอดำเนินการ</div></div>'
    +'<div class="sc sgn"><div class="snum" style="color:var(--ok)">'+retCount+'</div><div class="slbl">คืนแล้ว</div></div>'
    +'<div class="sc sorg"><div class="snum" style="color:#f59e0b">'+accPendingCount+'</div><div class="slbl">คืน (อุปกรณ์ค้าง)</div></div>'
    +'<div class="sc srd"><div class="snum" style="color:var(--er)">'+overMonth+'</div><div class="slbl">ยืมเกิน 1 เดือน</div></div>'
    +'<div class="sc spu"><div class="snum" style="color:var(--ac2)">'+exp.length+'</div><div class="slbl">รายการทั้งหมด</div></div>'
    +'</div>';

  // ── equipment bar chart ──
  var eqBarHtml='<div class="card" style="margin-bottom:12px"><div class="ctitle">📊 การใช้งานเครื่องมือ (กำลังยืม)</div><div class="bwrap">'
    +eqB.map(function(e){return'<div class="brow"><div class="blbl" title="'+e[0]+'">'+e[0]+'</div><div class="btrk"><div class="bfil" style="width:'+(e[1]/mA*100)+'%"></div></div><div class="bval">'+e[1]+'</div></div>';}).join('')
    +(eqB.length===0?'<div style="color:var(--mu);font-size:12px;padding:8px">ไม่มีรายการกำลังยืม</div>':'')
    +'</div></div>';

  // ── dept cross table ──
  var deptTableHtml='<div class="card" style="margin-bottom:12px"><div class="ctitle">🏥 การใช้งานแยกตามแผนก</div>';
  if(!deptList.length){
    deptTableHtml+='<div style="text-align:center;padding:20px;color:var(--mu)">ไม่มีรายการกำลังยืม</div>';
  }else{
    deptTableHtml+='<div class="dtw"><table class="dt"><thead><tr>'
      +'<th style="min-width:90px">แผนก</th>'
      +eqNames.map(function(n){return'<th style="text-align:center;white-space:normal;max-width:80px;font-size:9px">'+n+'</th>';}).join('')
      +'<th style="text-align:center;min-width:50px">รวม</th>'
      +'</tr></thead><tbody>'
      +deptList.map(function(dept){
        var total=Object.values(deptMap[dept]).reduce(function(s,v){return s+v;},0);
        return'<tr>'
          +'<td style="font-weight:600">'+dept+'</td>'
          +eqNames.map(function(n){var cnt=deptMap[dept][n]||0;return'<td style="text-align:center;font-family:var(--mo)'+(cnt>0?';color:var(--ac);font-weight:700':';color:var(--mu)')+'">'+(cnt>0?cnt:'-')+'</td>';}).join('')
          +'<td style="text-align:center;font-family:var(--mo);font-weight:700;color:var(--wn)">'+total+'</td>'
          +'</tr>';
      }).join('')
      +'</tbody></table></div>';
  }
  deptTableHtml+='</div>';

  // ── HFNC section ──
  var warnBanners='';
  if(warnAll){warnBanners+='<div class="hwarn"><span style="font-size:22px">🚨</span><div><strong style="color:var(--er);font-size:14px">แจ้งเตือนด่วน!</strong><br><span style="font-size:13px">รวมทุกอาคาร = <strong style="color:var(--er);font-size:16px">'+hfncTotal+'</strong> เครื่อง <strong style="color:var(--er)">เกินเกณฑ์ '+LIMIT_ALL+' เครื่อง!</strong></span></div></div>';}
  if(warn100){warnBanners+='<div style="background:rgba(239,68,68,.08);border:1px solid var(--er);border-radius:7px;padding:10px;display:flex;align-items:center;gap:8px;margin-bottom:10px"><span style="font-size:22px">⚠️</span><div><strong style="color:var(--er);font-size:13px">แจ้งเตือน อาคาร 100 ปี</strong><br><span style="font-size:12px">HFNC อาคาร 100 ปี = <strong style="color:var(--er)">'+b100+'</strong> เครื่อง — เกินเกณฑ์ '+LIMIT_100+' เครื่อง!</span></div></div>';}
  if(!warnAll&&!warn100){warnBanners='<div style="background:rgba(16,185,129,.08);border:1px solid var(--ok);border-radius:6px;padding:8px 12px;margin-bottom:10px;display:flex;gap:8px;align-items:center"><span style="font-size:18px">✅</span><div style="font-size:13px">ยอดรวม HFNC ทุกอาคาร = <strong style="color:var(--ok)">'+hfncTotal+'</strong> เครื่อง — ปกติ</div></div>';}
  var hfncHtml='<div class="card"><div class="ctitle">📊 รายงานการใช้งาน HFNC</div>'
    +warnBanners
    +'<div class="sgrid" style="margin-bottom:12px">'
    +'<div class="sc '+(warnAll?'srd':'scy')+'"><div class="snum" style="color:'+(warnAll?'var(--er)':'var(--ac)')+'">'+hfncTotal+'</div><div class="slbl">HFNC รวมทุกอาคาร</div><div style="font-size:10px;color:'+(warnAll?'var(--er)':'var(--mu)')+'">เกณฑ์สูงสุด '+LIMIT_ALL+' เครื่อง</div></div>'
    +(maxDept?'<div class="sc sgn"><div class="snum" style="color:var(--ok);font-size:17px">'+maxDept[0]+'</div><div class="slbl">ใช้สูงสุด ('+maxDept[1]+')</div></div>':'')
    +(minDept&&minDept!==maxDept?'<div class="sc spu"><div class="snum" style="color:var(--ac2);font-size:17px">'+minDept[0]+'</div><div class="slbl">ใช้ต่ำสุด ('+minDept[1]+')</div></div>':'')
    +'</div>'
    +'<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px">'
    +'<div><div style="font-size:10px;color:var(--mu);font-weight:700;text-transform:uppercase;margin-bottom:7px;letter-spacing:.5px">🏢 HFNC ตามอาคาร</div>'
    +'<div class="bwrap">'
    +bE.map(function(e){
      var isWb=wbList.indexOf(e[0])>=0,is100=e[0]==='อาคาร 100 ปี';
      var overLimit=(is100&&e[1]>LIMIT_100)||(isWb&&warnAll);
      var barColor=overLimit?'var(--er)':(isWb?'var(--wn)':'var(--ac)');
      var valColor=overLimit?'var(--er)':(isWb?'var(--wn)':'var(--tx)');
      var limitTxt=is100?' /'+LIMIT_100:'';
      return'<div class="brow"><div class="blbl" title="'+e[0]+'">'+e[0]+'</div><div class="btrk"><div class="bfil" style="width:'+(e[1]/mB*100)+'%;background:'+barColor+'"></div></div><div class="bval" style="color:'+valColor+'">'+e[1]+limitTxt+'</div>'+(overLimit?'<span style="font-size:10px;color:var(--er);margin-left:3px">⚠️</span>':'')+'</div>';
    }).join('')
    +(bE.length===0?'<div style="color:var(--mu);font-size:12px;padding:8px">ไม่มีข้อมูล HFNC</div>':'')
    +'</div>'
    +'<div style="margin-top:8px;padding:8px 10px;background:var(--s2);border-radius:5px;font-size:12px;line-height:1.8">'
    +'<div style="color:var(--mu);font-weight:700;margin-bottom:4px;font-size:10px;text-transform:uppercase;letter-spacing:.4px">เกณฑ์การใช้งาน HFNC</div>'
    +'<div>🏥 อาคาร 100 ปี ใช้ได้ไม่เกิน <strong style="color:'+(warn100?'var(--er)':'var(--wn)')+'">'+LIMIT_100+' เครื่อง</strong>'+(warn100?' <span style="color:var(--er);font-weight:700">⚠️ เกิน!</span>':' <span style="color:var(--ok)">✓</span>')+'</div>'
    +'<div>🏢 รวม 4 อาคาร ไม่เกิน <strong style="color:'+(warnAll?'var(--er)':'var(--wn)')+'">'+LIMIT_ALL+' เครื่อง</strong>'+(warnAll?' <span style="color:var(--er);font-weight:700">⚠️ เกิน!</span>':' <span style="color:var(--ok)">✓ ปัจจุบัน '+hfncTotal+' เครื่อง</span>')+'</div>'
    +'</div></div>'
    +'<div><div style="font-size:10px;color:var(--mu);font-weight:700;text-transform:uppercase;margin-bottom:7px;letter-spacing:.5px">🏥 HFNC ตามหอผู้ป่วย</div>'
    +'<div class="bwrap">'+dE.map(function(e){return'<div class="brow"><div class="blbl">'+e[0]+'</div><div class="btrk"><div class="bfil" style="width:'+(e[1]/mHD*100)+'%;background:var(--ac2)"></div></div><div class="bval">'+e[1]+'</div></div>';}).join('')+'</div>'
    +'</div></div></div>';

  cont.innerHTML=statsHtml+eqBarHtml+deptTableHtml+hfncHtml;
}


// ============================================================
// SUMMARY
// ============================================================
async function loadSum(){
  initSumFilters();
  updateSumFilterBadges();
  // ถ้า filter วันที่เริ่มต้นเกินกว่า 1 ปีที่แล้ว → ดึงข้อมูลเพิ่ม
  var sumS=document.getElementById('sumS');
  var cutoff=new Date();cutoff.setFullYear(cutoff.getFullYear()-1);
  if(sumS&&sumS.value&&sumS.value<cutoff.toISOString().split('T')[0]){
    var[ld,bd]=await Promise.all([API.getAllLoans(true),API.getAllBorrows(true)]);
    loans=ld;borrows=bd;
  }
  await renderSum({avgUsage:{},overMonth:[],equipDays:{},circuitChange:[]});
}

// ── Timezone Bangkok (UTC+7) helpers ──────────────────────────
var _bkkFmt=new Intl.DateTimeFormat('en-CA',{timeZone:'Asia/Bangkok'});
function localDateStr(d){
  // ล็อค Asia/Bangkok เสมอ ไม่ขึ้นกับ timezone ของเครื่องผู้ใช้
  return _bkkFmt.format(d instanceof Date?d:new Date(d));
}
function bangkokNow(){return new Date();}
async function renderSum(r){
  var now=new Date(),sc=document.getElementById('sumCont');var _sMap={};
  if(!loans){sc.innerHTML='<div class="lding"><div class="spin"></div> กำลังโหลด...</div>';return Promise.resolve();}

  // ─── อ่าน filter ───────────────────────────────────────────
  var fDept =(document.getElementById('sumDept') ||{}).value||'';
  var fEq   =(document.getElementById('sumEq')   ||{}).value||'';
  var fStat =(document.getElementById('sumStat') ||{}).value||'';
  var fDateS=(document.getElementById('sumS')    ||{}).value||'';
  var fDateE=(document.getElementById('sumE')    ||{}).value||'';

  // ── Section Override filters ──────────────────────────
  var secBarEq  =(document.getElementById('secBarEq') ||{}).value||'';
  var secDayEq  =(document.getElementById('secDayEq') ||{}).value||'';
  var secAvgS   =(document.getElementById('secAvgS')  ||{}).value||'';
  var secAvgE   =(document.getElementById('secAvgE')  ||{}).value||'';
  var secAvgEq  =(document.getElementById('secAvgEq') ||{}).value||'';
  var secOvDept =(document.getElementById('secOvDept')||{}).value||'';
  var secOvEq   =(document.getElementById('secOvEq')  ||{}).value||'';
  var secAllDept=(document.getElementById('secAllDept')||{}).value||'';
  var secAllEq  =(document.getElementById('secAllEq') ||{}).value||'';
  var secAllStat=(document.getElementById('secAllStat')||{}).value||'';
  function secV(sv,gv){return sv||gv;}

  // ─── กราฟแท่ง: ยืมแยกตามรายการเครื่องมือ (ACTIVE) ───
  var exp=expanded();
  // กรองตาม filter
  var expFiltered=exp.filter(function(item){
    if(fStat&&item.status!==fStat)return false;
    if(fDept&&S(item.loan.DeptName)!==fDept)return false;
    if(fEq&&S(item.eq.name)!==fEq)return false;
    var ld=item.loan.LoanDate||'';
    if(fDateS&&ld&&ld<fDateS)return false;
    if(fDateE&&ld&&ld>fDateE)return false;
    return true;
  });
  // กราฟแท่ง — ใช้ secBarEq override
  var expBar=secBarEq?expFiltered.filter(function(i){return S(i.eq.name)===secBarEq;}):expFiltered;
  var eqCount={};
  expBar.forEach(function(item){
    if(item.status==='ACTIVE'){
      var n=S(item.eq.name);eqCount[n]=(eqCount[n]||0)+1;
    }
  });
  var eqArr=Object.entries(eqCount).sort(function(a,b){return b[1]-a[1];});
  var maxEq=eqArr.length>0?eqArr[0][1]:1;
  var barColors=['#00c8ff','#7c3aed','#10b981','#f59e0b','#ef4444','#06b6d4','#8b5cf6','#34d399','#fbbf24','#f87171','#22d3ee','#a78bfa'];

  var chartHtml='<div class="card"><div class="ctitle" style="margin-bottom:12px">📊 รายการยืมแยกตามชนิดเครื่องมือแพทย์ <span style="font-size:11px;color:var(--ok);font-weight:600;background:rgba(16,185,129,.1);border-radius:4px;padding:1px 7px;text-transform:none;letter-spacing:0">กำลังยืม (ACTIVE)</span></div>';
  if(eqArr.length===0){
    chartHtml+='<div style="text-align:center;padding:20px;color:var(--mu)">ไม่มีข้อมูล</div>';
  }else{
    var bW=34,bGap=12,padL=160,padB=36,padT=16,chartH=220;
    var totalW=padL+(bW+bGap)*eqArr.length+20;
    var bars=eqArr.map(function(e,i){
      var bH=Math.round((e[1]/maxEq)*(chartH-padT-padB));
      var x=padL+i*(bW+bGap);var y=padT+(chartH-padT-padB)-bH;
      var col=barColors[i%barColors.length];
      return'<g class="bar-grp" data-eq="'+e[0]+'" style="cursor:pointer" onclick="sumFilterByEq(\''+e[0]+'\')">'
        +'<rect x="'+x+'" y="'+y+'" width="'+bW+'" height="'+bH+'" fill="'+col+'" rx="3" opacity=".85"/>'
        +'<rect x="'+x+'" y="'+y+'" width="'+bW+'" height="'+bH+'" fill="transparent" stroke="'+col+'" stroke-width="1" rx="3"/>'
        +'<text x="'+(x+bW/2)+'" y="'+(y-5)+'" text-anchor="middle" fill="'+col+'" font-size="12" font-weight="700" font-family="IBM Plex Mono,monospace">'+e[1]+'</text>'
        +'<text x="'+(x+bW/2)+'" y="'+(chartH-padB+14)+'" text-anchor="middle" fill="#8b949e" font-size="9" transform="rotate(-35,'+(x+bW/2)+','+(chartH-padB+14)+')">'+e[0].substring(0,12)+(e[0].length>12?'..':'')+'</text>'
        +'</g>';
    }).join('');
    chartHtml+='<div style="overflow-x:auto"><svg width="'+totalW+'" height="'+chartH+'" xmlns="http://www.w3.org/2000/svg" style="display:block">'
      +'<line x1="'+padL+'" y1="'+padT+'" x2="'+padL+'" y2="'+(chartH-padB)+'" stroke="#30363d" stroke-width="1"/>'
      +'<line x1="'+padL+'" y1="'+(chartH-padB)+'" x2="'+(totalW-10)+'" y2="'+(chartH-padB)+'" stroke="#30363d" stroke-width="1"/>'
      +[0,25,50,75,100].map(function(pct){
        var yy=padT+(chartH-padT-padB)*(1-pct/100);var val=Math.round(maxEq*pct/100);
        return'<line x1="'+(padL-4)+'" y1="'+yy+'" x2="'+(totalW-10)+'" y2="'+yy+'" stroke="#30363d" stroke-width="0.5" stroke-dasharray="3,3"/>'
          +'<text x="'+(padL-7)+'" y="'+(yy+4)+'" text-anchor="end" fill="#8b949e" font-size="10">'+val+'</text>';
      }).join('')
      +bars+'</svg></div>'
      +'<div style="margin-top:8px;font-size:11px;color:var(--mu)">💡 คลิกที่แท่งกราฟเพื่อกรองข้อมูลในหน้าข้อมูลทั้งหมด | เลื่อนเมาส์เพื่อดู tooltip</div></div>';
  }

  // ─── ตารางยืมคืนรายวัน — ใช้ secDayEq override ───
  var expDay=secDayEq?expFiltered.filter(function(i){return S(i.eq.name)===secDayEq;}):expFiltered;
  var dayMap={};var allEqNames2={};
  expDay.forEach(function(item){
    if(!item.loan.LoanDate)return;
    var d=item.loan.LoanDate;
    if(!dayMap[d])dayMap[d]={borrow:{},ret:{},accPend:{}};
    var n=S(item.eq.name);allEqNames2[n]=true;
    if(item.status==='ACTIVE'||item.status==='ACC_PENDING'||item.status==='RETURNED'){
      dayMap[d].borrow[n]=(dayMap[d].borrow[n]||0)+1;
    }
    if(item.status==='RETURNED'||item.status==='ACC_PENDING'){
      if(item.borrow&&item.borrow.ReturnDate){
        var rd=item.borrow.ReturnDate;
        if(!dayMap[rd])dayMap[rd]={borrow:{},ret:{},accPend:{}};
        dayMap[rd].ret[n]=(dayMap[rd].ret[n]||0)+1;
      }
    }
  });
  var eqNames2=Object.keys(allEqNames2).sort();
  var dayList=Object.keys(dayMap).sort().reverse();
  // ── day section: bar chart + collapsible table ──
  var _dayUID='daytbl_'+Math.random().toString(36).substr(2,6);
  var dayTableHtml='<div class="card">';
  dayTableHtml+='<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">'
    +'<div class="ctitle" style="margin:0">📅 รายการยืม-คืนรายวัน แยกตามชนิดเครื่องมือ</div>'
    +'<button id="'+_dayUID+'_btn" onclick="(function(){var p=document.getElementById(\''+_dayUID+'\');var b=document.getElementById(\''+_dayUID+'_btn\');if(p.style.display===\'none\'){p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';}else{p.style.display=\'none\';b.textContent=\'▼ ดูตาราง\';}})();" class="btn bg sm" style="font-size:11px">▼ ดูตาราง</button>'
    +'</div>';
  if(dayList.length===0){
    dayTableHtml+='<div style="text-align:center;padding:20px;color:var(--mu)">ไม่มีข้อมูล</div>';
  }else{
    var _dShow=dayList.slice(0,30);
    var _dMaxB=Math.max.apply(null,_dShow.map(function(d){return Object.values(dayMap[d].borrow).reduce(function(s,v){return s+v;},0);}).concat([1]));
    var _dMaxR=Math.max.apply(null,_dShow.map(function(d){return Object.values(dayMap[d].ret).reduce(function(s,v){return s+v;},0);}).concat([1]));
    var _dMax=Math.max(_dMaxB,_dMaxR,1);
    var _bH=60,_bW=Math.max(18,Math.min(34,Math.floor(680/_dShow.length)));
    var _svgW=_dShow.length*(_bW+4)+60;
    var _bars=_dShow.slice().reverse().map(function(d,i){
      var totalB=Object.values(dayMap[d].borrow).reduce(function(s,v){return s+v;},0);
      var totalR=Object.values(dayMap[d].ret).reduce(function(s,v){return s+v;},0);
      var hB=Math.round(totalB/_dMax*_bH);var hR=Math.round(totalR/_dMax*_bH);
      var x=50+i*(_bW+4);var lbl=d.substring(5);
      return'<g style="cursor:pointer" onclick="sumFilterByDate(\''+d+'\')"><title>'+d+' ยืม:'+totalB+' คืน:'+totalR+'</title>'
        +(hB>0?'<rect x="'+x+'" y="'+(8+_bH-hB)+'" width="'+Math.floor(_bW*0.52)+'" height="'+hB+'" fill="#10b981" rx="2" opacity=".85"/>':'')
        +(hR>0?'<rect x="'+(x+Math.floor(_bW*0.54))+'" y="'+(8+_bH-hR)+'" width="'+Math.floor(_bW*0.4)+'" height="'+hR+'" fill="#8b949e" rx="2" opacity=".7"/>':'')
        +(totalB>0?'<text x="'+(x+Math.floor(_bW*0.26))+'" y="'+(5+_bH-hB)+'" text-anchor="middle" fill="#10b981" font-size="9" font-weight="700" font-family="IBM Plex Mono,monospace">'+totalB+'</text>':'')
        +'<text x="'+(x+Math.floor(_bW*0.46))+'" y="'+(8+_bH+13)+'" text-anchor="middle" fill="#8b949e" font-size="8" transform="rotate(-45,'+(x+Math.floor(_bW*0.46))+','+(8+_bH+13)+')">'+lbl+'</text>'
        +'</g>';
    }).join('');
    dayTableHtml+='<div style="overflow-x:auto;margin-bottom:6px">'
      +'<svg width="'+_svgW+'" height="'+(8+_bH+40)+'" xmlns="http://www.w3.org/2000/svg" style="display:block">'
      +'<line x1="46" y1="8" x2="46" y2="'+(8+_bH)+'" stroke="#30363d" stroke-width="1"/>'
      +'<line x1="46" y1="'+(8+_bH)+'" x2="'+(_svgW-2)+'" y2="'+(8+_bH)+'" stroke="#30363d" stroke-width="1"/>'
      +[0,25,50,75,100].map(function(p){var yy=8+_bH-Math.round(p/100*_bH);var v=Math.round(_dMax*p/100);return'<text x="43" y="'+(yy+3)+'" text-anchor="end" fill="#8b949e" font-size="9">'+v+'</text><line x1="46" y1="'+yy+'" x2="'+(_svgW-2)+'" y2="'+yy+'" stroke="#30363d" stroke-width="0.4" stroke-dasharray="2,3"/>';}).join('')
      +_bars+'</svg></div>'
      +'<div style="display:flex;gap:12px;font-size:11px;color:var(--mu);margin-bottom:4px;padding-left:50px">'
      +'<span><span style="display:inline-block;width:10px;height:10px;background:#10b981;border-radius:2px;margin-right:4px;vertical-align:middle"></span>ยืม</span>'
      +'<span><span style="display:inline-block;width:10px;height:10px;background:#8b949e;border-radius:2px;margin-right:4px;vertical-align:middle"></span>คืน</span>'
      +'<span style="color:var(--ac)">💡 คลิกแท่งเพื่อกรองวันนั้น | กด "ดูตาราง" เพื่อดูรายละเอียด</span>'
      +'</div>';
    dayTableHtml+='<div id="'+_dayUID+'" style="display:none">'
      +'<div class="dtw"><table class="dt"><thead><tr><th>วันที่</th>'
      +eqNames2.map(function(n){return'<th style="text-align:center;white-space:normal;max-width:70px;font-size:9px">'+n+'<br><span style="color:var(--ok)">ยืม</span>/<span style="color:var(--mu)">คืน</span></th>';}).join('')
      +'<th style="text-align:center">รวมยืม</th><th style="text-align:center">รวมคืน</th></tr></thead><tbody>'
      +_dShow.map(function(d){
        var totalB=Object.values(dayMap[d].borrow).reduce(function(s,v){return s+v;},0);
        var totalR=Object.values(dayMap[d].ret).reduce(function(s,v){return s+v;},0);
        return'<tr>'
          +'<td style="font-weight:600;white-space:nowrap"><button class="abt" style="border-color:var(--ac);color:var(--ac);font-size:11px" onclick="sumFilterByDate(\''+d+'\')">'+d+'</button></td>'
          +eqNames2.map(function(n){var b=dayMap[d].borrow[n]||0;var rt=dayMap[d].ret[n]||0;if(b===0&&rt===0)return'<td style="text-align:center;color:var(--mu)">-</td>';return'<td style="text-align:center">'+(b>0?'<span style="color:var(--ok);font-weight:700;font-family:var(--mo)">'+b+'</span>':'<span style="color:var(--mu)">0</span>')+'/'+(rt>0?'<span style="color:var(--mu);font-family:var(--mo)">'+rt+'</span>':'<span style="color:var(--mu)">0</span>')+'</td>';}).join('')
          +'<td style="text-align:center;font-family:var(--mo);font-weight:700;color:var(--ok)">'+totalB+'</td>'
          +'<td style="text-align:center;font-family:var(--mo);color:var(--mu)">'+totalR+'</td>'
          +'</tr>';
      }).join('')
      +'</tbody></table></div>'
      +(dayList.length>30?'<div style="font-size:11px;color:var(--mu);margin-top:6px;text-align:center">แสดง 30 วันล่าสุด</div>':'')
      +'</div>';
  }
  dayTableHtml+='</div>';

  var sumS_el=document.getElementById('sumS'),sumE_el=document.getElementById('sumE');
  var rangeS=sumS_el?sumS_el.value:'',rangeE=sumE_el?sumE_el.value:'';
  // avg section — ใช้ secAvgS/E/Eq override
  var expAvg=expFiltered;
  if(secAvgEq)expAvg=expAvg.filter(function(i){return S(i.eq.name)===secAvgEq;});
  var fAvgS=secAvgS||fDateS;
  var fAvgE=secAvgE||fDateE;
  if(secAvgS||secAvgE){
    expAvg=expAvg.filter(function(item){
      var ld=item.loan.LoanDate||'';
      if(fAvgS&&ld&&ld<fAvgS)return false;
      if(fAvgE&&ld&&ld>fAvgE)return false;
      return true;
    });
  }
  var allBorrowDates=[];
  expAvg.forEach(function(item){
    if(item.status==='PENDING'||item.status==='CANCELLED')return;
    if(item.loan.LoanDate)allBorrowDates.push(item.loan.LoanDate);
  });
  allBorrowDates.sort();
  var computeRangeS=rangeS||(allBorrowDates.length>0?allBorrowDates[0]:'');
  var computeRangeE=rangeE||localDateStr(now);
  if(!computeRangeS)computeRangeS=computeRangeE;
  var rangeStart=new Date(computeRangeS);
  var rangeEnd=new Date(computeRangeE);
  var totalDays=Math.max(1,Math.round((rangeEnd-rangeStart)/86400000)+1);
  var rangeDates=[];
  for(var dd=new Date(rangeStart);dd<=rangeEnd;dd.setDate(dd.getDate()+1)){
    rangeDates.push(localDateStr(dd));
  }
  var dailyActiveByEq={};
  var datesForAvg=rangeDates.slice().reverse().slice(0,30);

  // ── โหลด daily_snapshots จาก Supabase (ถ้ามี) ────────────────
  // ใช้ snapshot ก่อน ถ้าไม่มีค่อย fallback คำนวณเอง
  var _snapLoaded=false;
  try {
    var _snapQS='select=snapshot_date,equipment_name,active_count'
      +'&snapshot_date=gte.'+computeRangeS
      +'&snapshot_date=lte.'+computeRangeE
      +'&order=snapshot_date.asc&limit=2000';
    var _snapRes=await fetch(SUPABASE_URL+'/rest/v1/daily_snapshots?'+_snapQS,{
      headers:{'apikey':SUPABASE_ANON_KEY,'Authorization':'Bearer '+SUPABASE_ANON_KEY}
    });
    if(_snapRes.ok){
      var _snapData=await _snapRes.json();
      if(Array.isArray(_snapData)&&_snapData.length>0){
        _snapData.forEach(function(row){
          var n=row.equipment_name;
          var d=row.snapshot_date;
          if(!dailyActiveByEq[n])dailyActiveByEq[n]={};
          dailyActiveByEq[n][d]=row.active_count;
        });
        _snapLoaded=true;
        console.log('[renderSum] Using daily_snapshots:',_snapData.length,'rows');
      }
    }
  } catch(e){ console.warn('[renderSum] snapshot fetch failed, using fallback',e); }

  // ── Fallback: คำนวณจาก borrow_records ถ้าไม่มี snapshot ──────
  if(!_snapLoaded){
    console.log('[renderSum] No snapshots found, calculating from borrow_records');
    expAvg.forEach(function(item){
      if(item.status==='PENDING'||item.status==='CANCELLED')return;
      if(!item.loan.LoanDate)return;
      var n=S(item.eq.name);
      var loanD=item.loan.LoanDate;
      var todayLocal=localDateStr(now);
      var retD;
      if(item.status==='ACTIVE'||item.status==='ACC_PENDING'){
        retD=todayLocal;
      } else if(item.status==='RETURNED'){
        if(!item.borrow||!item.borrow.ReturnDate)return;
        retD=item.borrow.ReturnDate;
      } else { return; }
      rangeDates.forEach(function(d){
        if(d>=loanD&&d<=retD){
          if(!dailyActiveByEq[n])dailyActiveByEq[n]={};
          dailyActiveByEq[n][d]=(dailyActiveByEq[n][d]||0)+1;
        }
      });
    });
  }

  // ── วันนี้ใช้ข้อมูล live (expFiltered) เสมอ ──────────────────
  // เพราะ snapshot บันทึกตอนเที่ยงคืน แต่ข้อมูลอาจเปลี่ยนระหว่างวัน
  var todayStr2=localDateStr(now);
  expFiltered.forEach(function(item){
    if(item.status!=='ACTIVE'&&item.status!=='ACC_PENDING')return;
    var n=S(item.eq.name);
    if(!dailyActiveByEq[n])dailyActiveByEq[n]={};
    // override วันนี้ด้วย live count เสมอ
    if(!dailyActiveByEq[n][todayStr2])dailyActiveByEq[n][todayStr2]=0;
  });
  // นับ live count วันนี้
  var _liveToday={};
  expFiltered.forEach(function(item){
    if(item.status!=='ACTIVE'&&item.status!=='ACC_PENDING')return;
    var n=S(item.eq.name);
    _liveToday[n]=(_liveToday[n]||0)+1;
  });
  Object.keys(_liveToday).forEach(function(n){
    if(!dailyActiveByEq[n])dailyActiveByEq[n]={};
    dailyActiveByEq[n][todayStr2]=_liveToday[n];
  });

    var eqNamesAvg=Object.keys(dailyActiveByEq).sort(function(a,b){
    var ta=Object.values(dailyActiveByEq[a]).reduce(function(s,v){return s+v;},0);
    var tb=Object.values(dailyActiveByEq[b]).reduce(function(s,v){return s+v;},0);
    return tb-ta;
  });

    // ══════════════════════════════════════════════════════
  // 📈 ค่าเฉลี่ย On Loan รายวัน (จาก Dashboard / daily_snapshots)
  // ══════════════════════════════════════════════════════
  var _avgEqFilter=secAvgEq||fEq||'';
  var _avgDateS=secAvgS||fDateS||'';
  var _avgDateE=secAvgE||fDateE||localDateStr(now);

  // build snapMap
  var snapMap={};
  Object.keys(dailyActiveByEq).forEach(function(eqName){
    if(_avgEqFilter&&eqName!==_avgEqFilter)return;
    var dayData=dailyActiveByEq[eqName]||{};
    Object.keys(dayData).forEach(function(d){
      if(_avgDateS&&d<_avgDateS)return;
      if(_avgDateE&&d>_avgDateE)return;
      if(!snapMap[eqName])snapMap[eqName]={};
      snapMap[eqName][d]=dayData[d];
    });
  });

  // รวมข้อมูล inventory จาก Dashboard (_EQ_INV)
  var _invFull={};
  if(window._EQ_INV){
    window._EQ_INV.forEach(function(eq){
      var n=(eq.name||'').trim();
      _invFull[n.toLowerCase()]={
        name:n,
        total:eq.total||0,
        active:Math.max(0,(eq.total||0)-(eq.inactive||0)),
        inactive:eq.inactive||0,
        forDept:eq.forDept||0
      };
    });
  }

  var snapEqNames=Object.keys(snapMap).sort(function(a,b){
    var sa=Object.values(snapMap[a]).reduce(function(s,v){return s+v;},0);
    var sb=Object.values(snapMap[b]).reduce(function(s,v){return s+v;},0);
    return sb-sa;
  });

  var BAR_COLORS2=['#00c8ff','#7c3aed','#10b981','#f59e0b','#ef4444','#06b6d4','#8b5cf6','#34d399','#fbbf24','#f87171','#22d3ee','#a78bfa','#fb923c','#4ade80','#c084fc'];

  var snapStats=snapEqNames.map(function(n,i){
    var dayData=snapMap[n]||{};
    var dates=Object.keys(dayData).sort();
    var totalDaysS=dates.length;
    var sumActiveS=dates.reduce(function(s,d){return s+(dayData[d]||0);},0);
    var avgS=totalDaysS>0?(sumActiveS/totalDaysS).toFixed(2):'0.00';
    var peakS=totalDaysS>0?Math.max.apply(null,dates.map(function(d){return dayData[d]||0;})):0;
    var todayVal=dayData[localDateStr(now)]||0;
    var last7=dates.slice(-7);
    var ma7S=last7.length>0?(last7.reduce(function(s,d){return s+(dayData[d]||0);},0)/last7.length).toFixed(2):'0.00';
    var trendS=parseFloat(ma7S)>parseFloat(avgS)?'up':parseFloat(ma7S)<parseFloat(avgS)?'down':'flat';
    var inv=_invFull[n.toLowerCase()]||{total:0,active:0,inactive:0,forDept:0};
    var utilPct=inv.active>0?parseFloat((todayVal/inv.active*100).toFixed(1)):null;
    var availableNow=Math.max(0,(inv.active||0)-(inv.forDept||0)-todayVal);
    var col=BAR_COLORS2[i%BAR_COLORS2.length];
    return{name:n,avg:avgS,peak:peakS,todayVal:todayVal,ma7:ma7S,
           sumActive:sumActiveS,totalDays:totalDaysS,trend:trendS,
           utilPct:utilPct,inv:inv,availableNow:availableNow,col:col};
  });

  var _bkkTime=new Intl.DateTimeFormat('th-TH',{timeZone:'Asia/Bangkok',hour:'2-digit',minute:'2-digit',second:'2-digit',hour12:false}).format(now);
  var _bkkDate=new Intl.DateTimeFormat('th-TH',{timeZone:'Asia/Bangkok',year:'numeric',month:'short',day:'numeric'}).format(now);
  var _avgRangeLabel=(_avgDateS||computeRangeS)+' ถึง '+(_avgDateE||computeRangeE);
  var _snapSource=_snapLoaded?'📸 daily_snapshots':'⚡ คำนวณ realtime';
  var _snapBadgeColor=_snapLoaded?'var(--ok)':'var(--wn)';

  var avgHtml='';
  if(snapStats.length===0){
    avgHtml='<div class="card"><div class="ctitle">📈 ค่าเฉลี่ย On Loan รายวัน (Dashboard)</div>'      +'<div style="text-align:center;padding:30px;color:var(--mu)">ไม่มีข้อมูล — '      +'<a href="backfill_180.html" target="_blank" style="color:var(--ac)">คลิกเพื่อสร้างข้อมูลย้อนหลัง</a></div></div>';
  }else{
    // summary cards
    var totalOnLoanToday=snapStats.reduce(function(s,e){return s+e.todayVal;},0);
    var totalAvg=snapStats.reduce(function(s,e){return s+parseFloat(e.avg);},0).toFixed(1);
    var maxDays=Math.max.apply(null,snapStats.map(function(e){return e.totalDays;}));
    var summaryHtml='<div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(130px,1fr));gap:8px;margin-bottom:14px">'      +'<div style="background:var(--s2);border-radius:6px;padding:10px 14px;text-align:center">'        +'<div style="font-size:10px;color:var(--mu);margin-bottom:3px">เครื่องมือ</div>'        +'<div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--tx)">'+snapStats.length+'</div>'        +'<div style="font-size:10px;color:var(--mu)">รายการ</div></div>'      +'<div style="background:rgba(0,200,255,.08);border:1px solid rgba(0,200,255,.2);border-radius:6px;padding:10px 14px;text-align:center">'        +'<div style="font-size:10px;color:var(--mu);margin-bottom:3px">On Loan วันนี้</div>'        +'<div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--ac)">'+totalOnLoanToday+'</div>'        +'<div style="font-size:10px;color:var(--mu)">เครื่อง</div></div>'      +'<div style="background:rgba(16,185,129,.08);border:1px solid rgba(16,185,129,.2);border-radius:6px;padding:10px 14px;text-align:center">'        +'<div style="font-size:10px;color:var(--mu);margin-bottom:3px">AVG รวม</div>'        +'<div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--ok)">'+totalAvg+'</div>'        +'<div style="font-size:10px;color:var(--mu)">เครื่อง/วัน</div></div>'      +'<div style="background:var(--s2);border-radius:6px;padding:10px 14px;text-align:center">'        +'<div style="font-size:10px;color:var(--mu);margin-bottom:3px">ข้อมูลสะสม</div>'        +'<div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--tx)">'+maxDays+'</div>'        +'<div style="font-size:10px;color:var(--mu)">วัน</div></div>'      +'</div>';

    var tableRowsS=snapStats.map(function(e){
      var trendTxt=e.trend==='up'?'▲':e.trend==='down'?'▼':'─';
      var trendC=e.trend==='up'?'var(--er)':e.trend==='down'?'var(--ok)':'var(--mu)';
      // utilization bar + %
      var utilCell=e.utilPct!==null
        ?('<div style="display:flex;flex-direction:column;align-items:center;gap:2px">'          +'<span style="font-size:12px;font-weight:700;color:'+(e.utilPct>=80?'var(--er)':e.utilPct>=60?'var(--wn)':'var(--ok)')+'">'+e.utilPct+'%</span>'          +'<div style="width:56px;height:4px;background:var(--bg);border-radius:2px;overflow:hidden">'            +'<div style="height:100%;width:'+Math.min(100,e.utilPct)+'%;background:'+(e.utilPct>=80?'var(--er)':e.utilPct>=60?'var(--wn)':'var(--ok)')+';border-radius:2px"></div>'          +'</div>'          +'<span style="font-size:9px;color:var(--mu)">'+e.todayVal+'/'+(e.inv.active||'?')+'</span>'          +'</div>')
        :'<span style="color:var(--mu);font-size:10px">—</span>';
      // sparkline 14 วัน
      var spDates=Object.keys(snapMap[e.name]||{}).sort().slice(-14);
      var spVals=spDates.map(function(d){return(snapMap[e.name]||{})[d]||0;});
      var spMax=Math.max.apply(null,spVals.concat([1]));
      var spW=70,spH=22;
      var spPts=spVals.map(function(v,i){return Math.round(i*(spW/(spVals.length-1||1)))+','+Math.round(spH-3-(v/spMax)*(spH-6));}).join(' ');
      var spFill=spVals.length>1?spVals.map(function(v,i){return Math.round(i*(spW/(spVals.length-1||1)))+','+Math.round(spH-3-(v/spMax)*(spH-6));}).join(' ')+' '+spW+','+(spH-1)+' 0,'+(spH-1):'';
      var spSvg='<svg width="'+spW+'" height="'+spH+'" style="vertical-align:middle">'        +(spFill?'<polyline points="'+spFill+'" fill="'+e.col+'" opacity="0.12"/>':'')
        +(spVals.length>1?'<polyline points="'+spPts+'" fill="none" stroke="'+e.col+'" stroke-width="1.5" stroke-linejoin="round"/>':'')
        +(spVals.length>0?'<circle cx="'+Math.round((spVals.length-1)*(spW/(spVals.length-1||1)))+'" cy="'+Math.round(spH-3-(spVals[spVals.length-1]||0)/spMax*(spH-6))+'" r="2.5" fill="'+e.col+'"/>':'')
        +'</svg>';
      var ak='snp_'+e.name.replace(/[^a-zA-Z0-9]/g,'_');_sMap[ak]={type:'eq',eq:e.name};
      return'<tr class="avg-tbl-row" style="border-bottom:1px solid var(--bd)">'        +'<td style="padding:8px 10px"><div style="display:flex;align-items:center;gap:7px">'          +'<span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:'+e.col+';flex-shrink:0"></span>'          +'<strong>'+e.name+'</strong>'          +'<span style="font-size:10px;color:'+trendC+'">'+trendTxt+'</span>'        +'</div></td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--mu)">'+( e.inv.total||'—')+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--ok);font-weight:700">'+( e.inv.active||'—')+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--er)">'+( e.inv.inactive||0)+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--wn)">'+( e.inv.forDept||0)+'</td>'        +'<td style="text-align:center;font-family:var(--mo);font-weight:700;font-size:15px;color:var(--ac)">'+e.todayVal+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--ok)">'+e.availableNow+'</td>'        +'<td style="text-align:center;font-family:var(--mo);font-weight:700;font-size:15px;color:var(--ac)">'+e.avg+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:'+(parseFloat(e.ma7)>parseFloat(e.avg)?'var(--er)':parseFloat(e.ma7)<parseFloat(e.avg)?'var(--ok)':'var(--ac2)')+'">'+e.ma7+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--wn)">'+e.peak+'</td>'        +'<td style="text-align:center;padding:6px 8px">'+utilCell+'</td>'        +'<td style="text-align:center;font-family:var(--mo);color:var(--mu);font-size:11px">'+e.totalDays+'</td>'        +'<td style="text-align:center">'+spSvg+'</td>'        +'<td style="text-align:center"><button class="abt svb" data-key="'+ak+'" style="border-color:var(--ac);color:var(--ac);font-size:11px">ดู</button></td>'        +'</tr>';
    }).join('');

    avgHtml='<div class="card">'      +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">'        +'<div>'          +'<div class="ctitle" style="margin:0;margin-bottom:2px">📈 ค่าเฉลี่ย On Loan รายวัน'            +'<span style="font-size:11px;color:var(--mu);font-weight:400;text-transform:none;letter-spacing:0;margin-left:8px">'+_avgRangeLabel+'</span></div>'          +'<div style="font-size:11px;color:var(--mu)">ข้อมูลจากหน้า Dashboard — บันทึกอัตโนมัติทุกวัน 23:59 น. Bangkok</div>'        +'</div>'        +'<div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap">'          +'<span style="font-size:11px;font-weight:700;color:'+_snapBadgeColor+'">'+_snapSource+'</span>'          +'<span style="color:var(--ac);font-family:var(--mo);font-size:11px">'+_bkkDate+' '+_bkkTime+'</span>'          +'<span style="font-size:10px;background:rgba(16,185,129,.1);border:1px solid rgba(16,185,129,.25);border-radius:10px;padding:1px 8px;color:var(--ok)">🔄 realtime</span>'        +'</div>'      +'</div>'      +summaryHtml      +'<div style="overflow-x:auto"><table class="dt" style="min-width:900px">'        +'<thead><tr style="background:var(--s2)">'          +'<th style="text-align:left;min-width:160px;font-size:11px">MEDICAL EQUIPMENT</th>'          +'<th style="text-align:center;font-size:11px">Total</th>'          +'<th style="text-align:center;font-size:11px;color:var(--ok)">Active<br>% of Total</th>'          +'<th style="text-align:center;font-size:11px;color:var(--er)">Inactive</th>'          +'<th style="text-align:center;font-size:11px;color:var(--wn)">For Dept Use</th>'          +'<th style="text-align:center;font-size:11px;color:var(--ac)">On Loan<br>วันนี้</th>'          +'<th style="text-align:center;font-size:11px;color:var(--ok)">Available<br>วันนี้</th>'          +'<th style="text-align:center;font-size:11px;color:var(--ac)">AVG On Loan<br><span style="font-weight:400">เครื่อง/วัน</span></th>'          +'<th style="text-align:center;font-size:11px;color:var(--ac2)">MA7</th>'          +'<th style="text-align:center;font-size:11px;color:var(--wn)">Peak</th>'          +'<th style="text-align:center;font-size:11px">Utilization<br><span style="font-weight:400">On Loan/Active</span></th>'          +'<th style="text-align:center;font-size:11px">วันที่มีข้อมูล</th>'          +'<th style="text-align:center;font-size:11px">แนวโน้ม (14วัน)</th>'          +'<th style="width:36px"></th>'        +'</tr></thead>'        +'<tbody>'+tableRowsS+'</tbody>'      +'</table></div>'      +'<div style="margin-top:10px;padding:8px 12px;background:var(--s2);border-radius:6px;font-size:11px;color:var(--mu);line-height:1.8">'        +'💡 <strong style="color:var(--ac)">AVG</strong> = ผลรวม On Loan แต่ละวัน ÷ จำนวนวันที่มีข้อมูล'        +' &nbsp;|&nbsp; <strong style="color:var(--ac2)">MA7</strong> = เฉลี่ย 7 วันล่าสุด (▲ เพิ่ม / ▼ ลด)'        +' &nbsp;|&nbsp; <strong style="color:var(--ok)">Available</strong> = Active − For Dept Use − On Loan (วันนี้)'        +' &nbsp;|&nbsp; ข้อมูล Total/Active/Inactive/ForDept จาก Admin → จัดการสถานะเครื่องมือ'      +'</div>'      +'</div>';
  }



  // ─── origHtml: ⏰ยืมเกิน 1 เดือน + 🔢วันที่ใช้งาน + 🔄Circuit ───
  var overMonthLocal=[];
  var _omLoans=fDept||fEq?expFiltered.map(function(i){return i.loan;}).filter(function(l,idx,arr){return arr.findIndex(function(x){return x.LoanID===l.LoanID;})===idx;}):loans;
  var _ovDept2=secOvDept||fDept;
  var _ovEq2=secOvEq||fEq;
  _omLoans.forEach(function(loan){
    if(loan.Status==='CANCELLED'||loan.Status==='RETURNED')return;
    var ld=loan.LoanDate;if(!ld)return;
    var daysUsed=Math.round((now-new Date(ld))/86400000);
    if(daysUsed>30){overMonthLocal.push({LoanID:loan.LoanID,LoanDate:ld,DeptName:loan.DeptName,PatientName:loan.PatientName,EquipmentList:loan.EquipmentList||[],days:daysUsed});}
  });
  overMonthLocal.sort(function(a,b){return b.days-a.days;});
  if(_ovDept2)overMonthLocal=overMonthLocal.filter(function(r){return S(r.DeptName)===_ovDept2;});
  if(_ovEq2)overMonthLocal=overMonthLocal.filter(function(r){return(r.EquipmentList||[]).some(function(e){return S(e.name)===_ovEq2;});});


  // ── section 2: ยืมเกิน 1 เดือน — bar chart + collapsible table ──
  var _omUID='omtbl_'+Math.random().toString(36).substr(2,6);
  (function(){
    var omBars='';
    if(overMonthLocal.length>0){
      var _omMax=overMonthLocal[0].days||1;
      var _omBW=Math.max(24,Math.min(48,Math.floor(640/Math.min(overMonthLocal.length,20))));
      var _omShow=overMonthLocal.slice(0,20);
      var _omSvgW=_omShow.length*(_omBW+4)+60;
      var _omBars=_omShow.map(function(row,i){
        var hB=Math.round(row.days/_omMax*60);
        var x=50+i*(_omBW+4);
        var col=row.days>=60?'#ef4444':'#f59e0b';
        var sk='s_'+S(row.LoanID);_sMap[sk]=row;
        var dept=S(row.DeptName).substring(0,6);
        return'<g style="cursor:pointer" onclick="(function(){var p=document.getElementById(\''+_omUID+'\');var b=document.getElementById(\''+_omUID+'_btn\');p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';})()"><title>'+S(row.DeptName)+' '+S(row.PatientName)+' '+row.days+' วัน</title>'
          +'<rect x="'+x+'" y="'+(8+60-hB)+'" width="'+_omBW+'" height="'+hB+'" fill="'+col+'" rx="3" opacity=".85"/>'
          +'<text x="'+(x+_omBW/2)+'" y="'+(5+60-hB)+'" text-anchor="middle" fill="'+col+'" font-size="9" font-weight="700" font-family="IBM Plex Mono,monospace">'+row.days+'</text>'
          +'<text x="'+(x+_omBW/2)+'" y="'+(8+60+13)+'" text-anchor="middle" fill="#8b949e" font-size="8" transform="rotate(-40,'+(x+_omBW/2)+','+(8+60+13)+')">'+dept+'</text>'
          +'</g>';
      }).join('');
      omBars='<div style="overflow-x:auto;margin-bottom:6px">'
        +'<svg width="'+_omSvgW+'" height="110" xmlns="http://www.w3.org/2000/svg" style="display:block">'
        +'<line x1="46" y1="8" x2="46" y2="68" stroke="#30363d" stroke-width="1"/>'
        +'<line x1="46" y1="68" x2="'+(_omSvgW-2)+'" y2="68" stroke="#30363d" stroke-width="1"/>'
        +[0,25,50,75,100].map(function(p){var yy=8+60-Math.round(p/100*60);var v=Math.round(_omMax*p/100);return'<text x="43" y="'+(yy+3)+'" text-anchor="end" fill="#8b949e" font-size="9">'+v+'</text><line x1="46" y1="'+yy+'" x2="'+(_omSvgW-2)+'" y2="'+yy+'" stroke="#30363d" stroke-width="0.4" stroke-dasharray="2,3"/>';}).join('')
        +_omBars
        +'<text x="5" y="40" fill="#f59e0b" font-size="9" font-weight="700" transform="rotate(-90,5,40)">วัน</text>'
        +'</svg></div>'
        +'<div style="font-size:11px;color:var(--mu);margin-bottom:4px;padding-left:50px">'
        +'<span style="color:#ef4444">🔴 ≥60 วัน</span>  <span style="color:#f59e0b">🟡 31-59 วัน</span>  <span style="color:var(--ac)">💡 คลิกแท่งเพื่อดูตาราง</span>'
        +'</div>';
    }
    var omTable=overMonthLocal.length
      ?'<div class="dtw"><table class="dt"><thead><tr><th>LoanID</th><th>วันที่ยืม</th><th>แผนก</th><th>ผู้ป่วย</th><th>เครื่องมือ</th><th style="text-align:center">จำนวนวัน</th><th style="text-align:center">รายละเอียด</th></tr></thead><tbody>'
        +overMonthLocal.map(function(row){var sk='s_'+S(row.LoanID);_sMap[sk]=row;var dC=row.days>60?'var(--er)':'var(--wn)';return'<tr><td style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+S(row.LoanID)+'</td><td>'+row.LoanDate+'</td><td><strong>'+S(row.DeptName)+'</strong></td><td>'+S(row.PatientName)+'</td><td>'+(row.EquipmentList||[]).map(function(e){return e.name;}).join(', ')+'</td><td style="text-align:center;font-family:var(--mo);font-weight:700;color:'+dC+'">'+row.days+' วัน</td><td style="text-align:center"><button class="abt svb" data-key="'+sk+'" style="border-color:var(--ac);color:var(--ac);font-size:11px">รายละเอียด</button></td></tr>';}).join('')
        +'</tbody></table></div>'
      :'<p style="color:var(--ok);text-align:center;padding:14px">✅ ไม่มีรายการเกิน 1 เดือน</p>';

    window._omHtml='<div class="card">'
      +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">'
        +'<div class="ctitle" style="margin:0">⏰ รายการยืมเกิน 1 เดือน <span style="font-size:11px;color:'+(overMonthLocal.length>0?'var(--er)':'var(--ok)')+';font-weight:600;text-transform:none;letter-spacing:0">'+(overMonthLocal.length>0?overMonthLocal.length+' รายการ':'ปกติ')+'</span></div>'
        +(overMonthLocal.length?'<button id="'+_omUID+'_btn" onclick="(function(){var p=document.getElementById(\''+_omUID+'\');var b=document.getElementById(\''+_omUID+'_btn\');if(p.style.display===\'none\'){p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';}else{p.style.display=\'none\';b.textContent=\'▼ ดูตาราง\';}})();" class="btn bg sm" style="font-size:11px">▼ ดูตาราง</button>':'')
      +'</div>'
      +omBars
      +'<div id="'+_omUID+'" style="display:none">'+omTable+'</div>'
      +'</div>';
  })();

  // ── section 3: วันที่ใช้งาน — bar chart + collapsible table ──
  var _eqdUID='eqdtbl_'+Math.random().toString(36).substr(2,6);
  (function(){
    var eqDaysLocal={};
    expFiltered.forEach(function(item){
      if(item.status==='PENDING'||item.status==='CANCELLED')return;
      if(!item.borrow)return;
      var eqNo=S(item.borrow.EquipmentNo);
      if(!eqNo||eqNo==='—'||eqNo==='-'||eqNo==='')return;
      var ld=item.loan.LoanDate;if(!ld)return;
      var startD=new Date(ld);
      var endD=item.borrow.ReturnDate?new Date(item.borrow.ReturnDate):now;
      var days=Math.max(1,Math.round((endD-startD)/86400000));
      if(!eqDaysLocal[eqNo])eqDaysLocal[eqNo]={days:0,equipName:S(item.eq.name),loanCount:0,active:item.status==='ACTIVE'||item.status==='ACC_PENDING'};
      eqDaysLocal[eqNo].days+=days;eqDaysLocal[eqNo].loanCount+=1;
      if(item.status==='ACTIVE'||item.status==='ACC_PENDING')eqDaysLocal[eqNo].active=true;
      if(!eqDaysLocal[eqNo].equipName&&S(item.eq.name))eqDaysLocal[eqNo].equipName=S(item.eq.name);
    });
    var allEqDays=Object.entries(eqDaysLocal).sort(function(a,b){return b[1].days-a[1].days;});
    var totalEq=allEqDays.length;
    var _eqdShow=allEqDays.slice(0,20);
    var _eqdMax=_eqdShow.length>0?_eqdShow[0][1].days:1;
    var _eqdBW=Math.max(20,Math.min(40,Math.floor(660/Math.max(_eqdShow.length,1))));
    var _eqdSvgW=_eqdShow.length*(_eqdBW+4)+60;
    var eqdBars='';
    if(_eqdShow.length>0){
      var _bars2=_eqdShow.map(function(e,i){
        var hB=Math.round(e[1].days/_eqdMax*60);
        var x=50+i*(_eqdBW+4);
        var col=e[1].days>30?'#ef4444':e[1].days>14?'#f59e0b':'#10b981';
        var lbl=e[0].substring(0,6);
        return'<g style="cursor:pointer" onclick="(function(){var p=document.getElementById(\''+_eqdUID+'\');var b=document.getElementById(\''+_eqdUID+'_btn\');p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';var s=document.getElementById(\'eqDaysSearch\');if(s){s.value=\''+e[0].replace(/'/g,'\\\'').substring(0,10)+'\';filterEqDays(s.value);}})()"><title>'+e[0]+' ('+e[1].equipName+') '+e[1].days+' วัน '+e[1].loanCount+' ครั้ง</title>'
          +'<rect x="'+x+'" y="'+(8+60-hB)+'" width="'+_eqdBW+'" height="'+hB+'" fill="'+col+'" rx="3" opacity=".85"/>'
          +(e[1].active?'<rect x="'+x+'" y="8" width="'+_eqdBW+'" height="4" fill="#10b981" rx="1" opacity=".6"/>':'')
          +'<text x="'+(x+_eqdBW/2)+'" y="'+(5+60-hB)+'" text-anchor="middle" fill="'+col+'" font-size="9" font-weight="700" font-family="IBM Plex Mono,monospace">'+e[1].days+'</text>'
          +'<text x="'+(x+_eqdBW/2)+'" y="'+(8+60+13)+'" text-anchor="middle" fill="#8b949e" font-size="8" transform="rotate(-40,'+(x+_eqdBW/2)+','+(8+60+13)+')">'+lbl+'</text>'
          +'</g>';
      }).join('');
      eqdBars='<div style="overflow-x:auto;margin-bottom:6px">'
        +'<svg width="'+_eqdSvgW+'" height="110" xmlns="http://www.w3.org/2000/svg" style="display:block">'
        +'<line x1="46" y1="8" x2="46" y2="68" stroke="#30363d" stroke-width="1"/>'
        +'<line x1="46" y1="68" x2="'+(_eqdSvgW-2)+'" y2="68" stroke="#30363d" stroke-width="1"/>'
        +[0,25,50,75,100].map(function(p){var yy=8+60-Math.round(p/100*60);var v=Math.round(_eqdMax*p/100);return'<text x="43" y="'+(yy+3)+'" text-anchor="end" fill="#8b949e" font-size="9">'+v+'</text><line x1="46" y1="'+yy+'" x2="'+(_eqdSvgW-2)+'" y2="'+yy+'" stroke="#30363d" stroke-width="0.4" stroke-dasharray="2,3"/>';}).join('')
        +_bars2+'</svg></div>'
        +'<div style="font-size:11px;color:var(--mu);margin-bottom:4px;padding-left:50px">'
        +'<span style="color:#ef4444">🔴 >30 วัน</span>  <span style="color:#f59e0b">🟡 15-30 วัน</span>  <span style="color:#10b981">🟢 ≤14 วัน</span>  <span style="background:rgba(16,185,129,.2);border-radius:2px;padding:0 4px">▬ กำลังใช้</span>  <span style="color:var(--ac)">💡 คลิกแท่งเพื่อดูรายการ</span>'
        +'</div>'
        +'(แสดง '+_eqdShow.length+' อันดับแรก จากทั้งหมด '+totalEq+' เครื่อง)';
    }
    var eqdRows=allEqDays.map(function(e){
      var avg=e[1].loanCount>0?(e[1].days/e[1].loanCount).toFixed(1):'-';
      var dayColor=e[1].days>30?'var(--er)':e[1].days>14?'var(--wn)':'var(--ok)';
      var activeBadge=e[1].active?'<span style="font-size:9px;background:rgba(16,185,129,.15);color:var(--ok);border:1px solid rgba(16,185,129,.3);border-radius:3px;padding:1px 4px;margin-left:3px">กำลังใช้</span>':'';
      return'<tr class="eqdrow" data-eqno="'+e[0].toLowerCase()+'" data-eqname="'+e[1].equipName.toLowerCase()+'">'
        +'<td style="font-family:var(--mo);color:var(--ac);font-weight:700">'+e[0]+activeBadge+'</td>'
        +'<td>'+e[1].equipName+'</td>'
        +'<td style="text-align:center;font-family:var(--mo)">'+e[1].loanCount+'</td>'
        +'<td style="text-align:center;font-family:var(--mo);font-weight:700;color:'+dayColor+'">'+e[1].days+'</td>'
        +'<td style="text-align:center;font-family:var(--mo);color:var(--mu)">'+avg+'</td>'
        +'</tr>';
    }).join('');
    window._eqdHtml='<div class="card">'
      +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">'
        +'<div class="ctitle" style="margin:0">🔢 วันที่ใช้งานของเครื่องมือแต่ละเครื่อง</div>'
        +'<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap">'
          +'<span style="font-size:11px;color:var(--mu)">ทั้งหมด '+totalEq+' เครื่อง</span>'
          +'<button id="'+_eqdUID+'_btn" onclick="(function(){var p=document.getElementById(\''+_eqdUID+'\');var b=document.getElementById(\''+_eqdUID+'_btn\');if(p.style.display===\'none\'){p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';}else{p.style.display=\'none\';b.textContent=\'▼ ดูตาราง\';}})();" class="btn bg sm" style="font-size:11px">▼ ดูตาราง</button>'
        +'</div>'
      +'</div>'
      +eqdBars
      +'<div id="'+_eqdUID+'" style="display:none">'
        +'<div style="display:flex;align-items:center;gap:6px;margin-bottom:8px">'
          +'<input type="text" id="eqDaysSearch" placeholder="🔍 ค้นหาชื่อเครื่องมือ / หมายเลขเครื่อง..." oninput="filterEqDays(this.value)" style="width:280px;font-size:12px;padding:5px 9px;background:var(--bg);border:1px solid var(--bd);border-radius:5px;color:var(--tx)">'
          +'<span id="eqDaysCount" style="font-size:11px;color:var(--mu)"></span>'
        +'</div>'
        +'<div class="dtw"><table class="dt" id="eqDaysTbl"><thead><tr>'
          +'<th>หมายเลขเครื่อง</th><th>ชนิดเครื่องมือ</th>'
          +'<th style="text-align:center">จำนวนครั้งที่ยืม</th>'
          +'<th style="text-align:center">วันสะสม</th>'
          +'<th style="text-align:center">เฉลี่ย/ครั้ง (วัน)</th>'
        +'</tr></thead><tbody id="eqDaysTbody">'+eqdRows+'</tbody></table></div>'
        +(totalEq===0?'<p style="text-align:center;color:var(--mu);padding:16px">ไม่มีข้อมูล</p>':'')
        +'<div style="margin-top:6px;font-size:11px;color:var(--mu)">💡 วันสะสม = วันระหว่างวันยืม-วันคืน | <span style="color:var(--ok)">กำลังใช้</span> = ยังไม่คืน</div>'
      +'</div>'
      +'</div>';
  })();

  // ── section 4: Circuit — bar chart + collapsible table ──
  var _cirUID='cirtbl_'+Math.random().toString(36).substr(2,6);
  (function(){
    var CIRCUIT_EQ=['Ventilator','Ventilator transfer','HFNC','HFNC transfer'];
    var circuitLocal=[];
    expFiltered.forEach(function(item){
      if(item.status!=='ACTIVE'&&item.status!=='ACC_PENDING')return;
      var eqName=S(item.eq.name);
      if(!CIRCUIT_EQ.some(function(n){return eqName===n;}))return;
      var ld=item.loan.LoanDate;if(!ld)return;
      var d=Math.round((now-new Date(ld))/86400000);
      if(d>30){circuitLocal.push({LoanID:S(item.loan.LoanID),LoanDate:ld,DeptName:S(item.loan.DeptName),PatientName:S(item.loan.PatientName),eqName:eqName,eqNo:item.borrow?S(item.borrow.EquipmentNo||'—'):'—',days:d});}
    });
    circuitLocal.sort(function(a,b){return b.days-a.days;});
    var countBadge=circuitLocal.length>0
      ?'<span style="font-size:11px;color:var(--wn);font-weight:600;text-transform:none;letter-spacing:0;background:rgba(245,158,11,.12);border:1px solid rgba(245,158,11,.3);border-radius:4px;padding:1px 8px;margin-left:6px">⚠️ '+circuitLocal.length+' รายการ</span>'
      :'<span style="font-size:11px;color:var(--ok);font-weight:600;text-transform:none;letter-spacing:0;margin-left:6px">✅ ปกติ</span>';
    var cirBars='';
    if(circuitLocal.length>0){
      var _cirMax=circuitLocal[0].days||1;
      var _cirShow=circuitLocal.slice(0,20);
      var _cirBW=Math.max(22,Math.min(44,Math.floor(660/Math.max(_cirShow.length,1))));
      var _cirSvgW=_cirShow.length*(_cirBW+4)+60;
      var _cirBars=_cirShow.map(function(row,i){
        var hB=Math.round(row.days/_cirMax*60);
        var x=50+i*(_cirBW+4);
        var col=row.days>=45?'#ef4444':'#f59e0b';
        var lbl=S(row.eqNo).substring(0,6)||S(row.DeptName).substring(0,4);
        return'<g style="cursor:pointer" onclick="(function(){var p=document.getElementById(\''+_cirUID+'\');var b=document.getElementById(\''+_cirUID+'_btn\');p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';})()"><title>'+S(row.eqNo)+' '+S(row.DeptName)+' '+row.days+' วัน</title>'
          +'<rect x="'+x+'" y="'+(8+60-hB)+'" width="'+_cirBW+'" height="'+hB+'" fill="'+col+'" rx="3" opacity=".85"/>'
          +'<text x="'+(x+_cirBW/2)+'" y="'+(5+60-hB)+'" text-anchor="middle" fill="'+col+'" font-size="9" font-weight="700" font-family="IBM Plex Mono,monospace">'+row.days+'</text>'
          +'<text x="'+(x+_cirBW/2)+'" y="'+(8+60+13)+'" text-anchor="middle" fill="#8b949e" font-size="8" transform="rotate(-40,'+(x+_cirBW/2)+','+(8+60+13)+')">'+lbl+'</text>'
          +'</g>';
      }).join('');
      cirBars='<div style="overflow-x:auto;margin-bottom:6px">'
        +'<svg width="'+_cirSvgW+'" height="110" xmlns="http://www.w3.org/2000/svg" style="display:block">'
        +'<line x1="46" y1="8" x2="46" y2="68" stroke="#30363d" stroke-width="1"/>'
        +'<line x1="46" y1="68" x2="'+(_cirSvgW-2)+'" y2="68" stroke="#30363d" stroke-width="1"/>'
        +[0,25,50,75,100].map(function(p){var yy=8+60-Math.round(p/100*60);var v=Math.round(_cirMax*p/100);return'<text x="43" y="'+(yy+3)+'" text-anchor="end" fill="#8b949e" font-size="9">'+v+'</text><line x1="46" y1="'+yy+'" x2="'+(_cirSvgW-2)+'" y2="'+yy+'" stroke="#30363d" stroke-width="0.4" stroke-dasharray="2,3"/>';}).join('')
        +_cirBars+'</svg></div>'
        +'<div style="font-size:11px;color:var(--mu);margin-bottom:4px;padding-left:50px">'
        +'<span style="color:#ef4444">🔴 ≥45 วัน (เร่งด่วน)</span>  <span style="color:#f59e0b">🟡 31-44 วัน</span>  <span style="color:var(--ac)">💡 คลิกแท่งเพื่อดูตาราง</span>'
        +'</div>';
    }
    var cirTable=circuitLocal.length
      ?'<div class="dtw"><table class="dt"><thead><tr><th>LoanID</th><th>วันที่ยืม</th><th>แผนก</th><th>ผู้ป่วย</th><th>เครื่องมือ</th><th>หมายเลขเครื่อง</th><th style="text-align:center">วันที่ใช้</th><th style="text-align:center">รายละเอียด</th></tr></thead><tbody>'
        +circuitLocal.map(function(row){var dC=row.days>=45?'var(--er)':'var(--wn)';var urgency=row.days>=45?'🔴':'🟡';return'<tr class="cirow" data-lid="'+row.LoanID+'" style="background:rgba(245,158,11,.04)"><td style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+row.LoanID+'</td><td>'+row.LoanDate+'</td><td><strong>'+row.DeptName+'</strong></td><td>'+row.PatientName+'</td><td>'+row.eqName+'</td><td style="font-family:var(--mo);color:var(--ac)">'+row.eqNo+'</td><td style="font-family:var(--mo);color:'+dC+';text-align:center;font-weight:700">'+urgency+' '+row.days+' วัน</td><td style="text-align:center"><button class="abt cirbtn" style="border-color:var(--ac);color:var(--ac);font-size:11px">👁 ดู</button></td></tr>';}).join('')
        +'</tbody></table></div>'
      :'<p style="color:var(--ok);text-align:center;padding:14px">✅ ไม่มีรายการที่ต้องเปลี่ยน Circuit ในขณะนี้</p>';
    window._cirHtml='<div class="card">'
      +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">'
        +'<div class="ctitle" style="margin:0">🔄 Ventilator/HFNC ต้องเปลี่ยน Circuit (เกิน 30 วัน)'+countBadge+'</div>'
        +(circuitLocal.length?'<button id="'+_cirUID+'_btn" onclick="(function(){var p=document.getElementById(\''+_cirUID+'\');var b=document.getElementById(\''+_cirUID+'_btn\');if(p.style.display===\'none\'){p.style.display=\'\';b.textContent=\'▲ ซ่อนตาราง\';}else{p.style.display=\'none\';b.textContent=\'▼ ดูตาราง\';}})();" class="btn bg sm" style="font-size:11px">▼ ดูตาราง</button>':'')
      +'</div>'
      +cirBars
      +'<div id="'+_cirUID+'" style="display:none">'+cirTable+'</div>'
      +'</div>';
  })();

  var origHtml=(window._omHtml||'')+(window._eqdHtml||'')+(window._cirHtml||'');


  // ─── allLoansHtml ─────────────────────────────────────────
  var ALL_STAT_LABEL={PENDING:'รอดำเนินการจ่าย',ACTIVE:'ยืม',RETURNED:'คืน',ACC_PENDING:'คืน (อุปกรณ์ค้าง)',CANCELLED:'ยกเลิก'};
  var ALL_STAT_BADGE={PENDING:'bp2',ACTIVE:'ba',RETURNED:'br2',ACC_PENDING:'bap',CANCELLED:'bc2'};
  var _allDept=secV(secAllDept,fDept);
  var _allEq=secV(secAllEq,fEq);
  var _allStat=secV(secAllStat,fStat);
  var allLoansRows=expFiltered.filter(function(item){
    if(item.status==='CANCELLED')return false;
    if(_allDept&&S(item.loan.DeptName)!==_allDept)return false;
    if(_allEq&&S(item.eq.name)!==_allEq)return false;
    if(_allStat&&item.status!==_allStat)return false;
    return true;
  });
  allLoansRows=allLoansRows.slice().sort(function(a,b){return(b.loan.LoanDate||'').localeCompare(a.loan.LoanDate||'');});
  var alStat={PENDING:0,ACTIVE:0,RETURNED:0,ACC_PENDING:0};
  allLoansRows.forEach(function(item){if(alStat[item.status]!==undefined)alStat[item.status]++;});
  var eqCountAll={};
  allLoansRows.forEach(function(item){var n=S(item.eq.name);eqCountAll[n]=(eqCountAll[n]||0)+1;});
  var eqBarArr=Object.entries(eqCountAll).sort(function(a,b){return b[1]-a[1];});
  var maxBarVal=eqBarArr.length>0?eqBarArr[0][1]:1;
  var BAR_COLORS=['#00c8ff','#7c3aed','#10b981','#f59e0b','#ef4444','#06b6d4','#8b5cf6','#34d399','#fbbf24','#f87171','#22d3ee','#a78bfa','#fb923c','#4ade80','#c084fc','#fb7185','#38bdf8','#facc15','#e879f9','#a3e635'];
  var barChartHtml2='';
  if(eqBarArr.length){
    var barRows2=eqBarArr.map(function(e,i){
      var pct=maxBarVal>0?e[1]/maxBarVal*100:0;
      var col=BAR_COLORS[i%BAR_COLORS.length];
      return'<div class="al-bar-row" data-eq="'+e[0].replace(/"/g,'&quot;')+'" style="display:flex;align-items:center;gap:0;padding:3px 0;cursor:pointer;border-radius:4px;transition:background .12s" title="'+e[0]+': '+e[1]+' รายการ — คลิกเพื่อกรอง">'        +'<div style="width:180px;text-align:right;padding-right:12px;font-size:12px;color:var(--mu);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;flex-shrink:0">'+e[0]+'</div>'        +'<div style="flex:1;display:flex;align-items:center;gap:8px">'          +'<div style="height:22px;background:'+col+';border-radius:0 4px 4px 0;min-width:4px;width:'+Math.round(pct*0.6)+'px"></div>'          +'<span style="font-size:12px;font-weight:700;font-family:var(--mo);color:'+col+'">'+e[1]+'</span>'        +'</div></div>';
    }).join('');
    barChartHtml2='<div id="alBarCont" style="padding:8px 0">'+barRows2+'</div>'      +'<div style="font-size:11px;color:var(--mu);margin-top:4px;padding-left:180px">💡 คลิกที่แถวเพื่อกรองตารางด้านล่าง</div>';
  }
  var alFilters=[];
  if(fDateS||fDateE)alFilters.push('📅 '+(fDateS||'...')+' – '+(fDateE||'...'));
  if(fDept)alFilters.push('🏥 '+fDept);
  if(fEq)alFilters.push('🔧 '+fEq);
  if(fStat)alFilters.push('● '+(ALL_STAT_LABEL[fStat]||fStat));
  var alFilterBadge=alFilters.length?'<div style="display:flex;gap:6px;flex-wrap:wrap;margin-bottom:10px">'+alFilters.map(function(f){return'<span style="background:rgba(0,200,255,.08);border:1px solid rgba(0,200,255,.25);border-radius:12px;padding:2px 10px;font-size:11px;color:var(--ac)">'+f+'</span>';}).join('')+'</div>':'';
  var alSummaryHtml='<div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px">'    +'<div style="background:rgba(0,200,255,.08);border:1px solid rgba(0,200,255,.2);border-radius:6px;padding:8px 16px;text-align:center"><div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--ac)">'+allLoansRows.length+'</div><div style="font-size:10px;color:var(--mu)">ทั้งหมด</div></div>'    +'<div style="background:rgba(245,158,11,.08);border:1px solid rgba(245,158,11,.2);border-radius:6px;padding:8px 16px;text-align:center"><div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--wn)">'+alStat.PENDING+'</div><div style="font-size:10px;color:var(--mu)">รอดำเนินการ</div></div>'    +'<div style="background:rgba(16,185,129,.08);border:1px solid rgba(16,185,129,.2);border-radius:6px;padding:8px 16px;text-align:center"><div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--ok)">'+alStat.ACTIVE+'</div><div style="font-size:10px;color:var(--mu)">กำลังยืม</div></div>'    +'<div style="background:rgba(139,148,158,.08);border:1px solid rgba(139,148,158,.2);border-radius:6px;padding:8px 16px;text-align:center"><div style="font-size:22px;font-weight:700;font-family:var(--mo);color:var(--mu)">'+alStat.RETURNED+'</div><div style="font-size:10px;color:var(--mu)">คืนแล้ว</div></div>'    +'<div style="background:rgba(245,158,11,.08);border:1px solid rgba(245,158,11,.2);border-radius:6px;padding:8px 16px;text-align:center"><div style="font-size:22px;font-weight:700;font-family:var(--mo);color:#f59e0b">'+alStat.ACC_PENDING+'</div><div style="font-size:10px;color:var(--mu)">อุปกรณ์ค้าง</div></div>'    +'</div>';
  var allLoansHtml='<div class="card" style="margin-top:4px">'    +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">'      +'<div class="ctitle" style="margin:0">📋 ข้อมูลการยืมเครื่องมือทั้งหมด</div>'      +'<div style="display:flex;gap:4px">'        +'<button class="btn bg sm" onclick="dlPage(&quot;sum&quot;,&quot;csv&quot;)">CSV</button>'        +'<button class="btn bg sm" onclick="dlPage(&quot;sum&quot;,&quot;xlsx&quot;)">XLS</button>'        +'<button class="btn bg sm" onclick="dlPage(&quot;sum&quot;,&quot;pdf&quot;)">PDF</button>'      +'</div>'    +'</div>'    +alFilterBadge    +alSummaryHtml    +'<div class="card" style="background:var(--bg);border-color:rgba(0,200,255,.15);margin-bottom:14px;padding:14px 16px">'      +'<div style="font-size:11px;font-weight:700;color:var(--ac);text-transform:uppercase;letter-spacing:.5px;margin-bottom:10px">📊 จำนวนการยืมแยกตามเครื่องมือ</div>'      +barChartHtml2    +'</div>'    +'</div>';



  // ── HFNC set section: ใช้ function เพื่อ re-render ตาม date filter ──
  window._buildHfncSetHtml = function(filterDateS, filterDateE) {
    var SET_DEFS=[
      {
        id:'airvo2',label:'Airvo2',color:'#3266ad',bg:'rgba(50,102,173,.15)',
        match:function(no){var n=parseInt(no,10);return!isNaN(n)&&((n>=1&&n<=25)||(n>=36&&n<=38)||(n>=59&&n<=70));}
      },
      {
        id:'heyer',label:'Heyer',color:'#7c3aed',bg:'rgba(124,58,237,.15)',
        match:function(no){var n=parseInt(no,10);return!isNaN(n)&&((n>=26&&n<=35)||(n>=39&&n<=58));}
      },
      {
        id:'o2flo',label:'O2FLO',color:'#10b981',bg:'rgba(16,185,129,.15)',
        match:function(no){return/^O2FLO\d+$/i.test((no||'').trim());}
      },
      {
        id:'mek',label:'MEK',color:'#f59e0b',bg:'rgba(245,158,11,.15)',
        match:function(no){return/^MEK\d+$/i.test((no||'').trim());}
      }
    ];

    var setStats={};
    SET_DEFS.forEach(function(s){
      setStats[s.id]={machines:{},totalLoans:0,activeNow:0};
    });
    var unknownMachines={};

    expanded().forEach(function(item){
      if(S(item.eq.name)!=='HFNC')return;
      if(!item.borrow)return;
      if(item.status==='PENDING'||item.status==='CANCELLED')return;
      // filter ช่วงเวลา
      var ld=item.loan.LoanDate||'';
      if(filterDateS&&ld&&ld<filterDateS)return;
      if(filterDateE&&ld&&ld>filterDateE)return;

      var eqNo=S(item.borrow.EquipmentNo).trim();
      var dept=S(item.loan.DeptName);
      var isActive=item.status==='ACTIVE'||item.status==='ACC_PENDING';
      var matched=false;
      SET_DEFS.forEach(function(s){
        if(s.match(eqNo)){
          matched=true;
          var st=setStats[s.id];
          st.totalLoans++;
          if(isActive)st.activeNow++;
          if(!st.machines[eqNo])st.machines[eqNo]={count:0,active:false,dept:''};
          st.machines[eqNo].count++;
          if(isActive){st.machines[eqNo].active=true;st.machines[eqNo].dept=dept;}
        }
      });
      if(!matched&&eqNo&&eqNo!=='—'&&eqNo!=='-'){
        if(!unknownMachines[eqNo])unknownMachines[eqNo]=0;
        unknownMachines[eqNo]++;
      }
    });

    var totalActive=0,totalLoans=0;
    SET_DEFS.forEach(function(s){totalActive+=setStats[s.id].activeNow;totalLoans+=setStats[s.id].totalLoans;});

    var cards=SET_DEFS.map(function(s){
      var st=setStats[s.id];var machCount=Object.keys(st.machines).length;
      var pct=machCount>0?Math.round(st.activeNow/machCount*100):0;
      return'<div style="background:var(--s2);border:1px solid var(--bd);border-radius:8px;padding:12px 14px;border-top:3px solid '+s.color+'">'
        +'<div style="font-size:13px;font-weight:700;color:'+s.color+';margin-bottom:2px">'+s.label+'</div>'
        +'<div style="font-size:26px;font-weight:700;font-family:var(--mo);color:var(--tx)">'+st.activeNow+'</div>'
        +'<div style="font-size:11px;color:var(--mu)">กำลังยืมอยู่</div>'
        +'<div style="margin-top:6px;font-size:11px;color:var(--mu)">ยืมทั้งหมด <strong style="color:var(--tx)">'+st.totalLoans+'</strong> ครั้ง | <strong style="color:var(--tx)">'+machCount+'</strong> เครื่อง</div>'
        +'<div style="margin-top:8px;height:4px;background:var(--bg);border-radius:2px;overflow:hidden">'
          +'<div style="height:100%;width:'+pct+'%;background:'+s.color+';border-radius:2px;transition:width .5s"></div>'
        +'</div></div>';
    }).join('');

    var donutData=SET_DEFS.map(function(s){return{label:s.label,val:setStats[s.id].totalLoans,color:s.color};});
    var donutTotal=donutData.reduce(function(s,d){return s+d.val;},0)||1;
    var donutR=60,donutCx=75,donutCy=75,donutInner=38;
    var donutPaths='';var cumAngle=-Math.PI/2;
    donutData.forEach(function(d){
      if(d.val===0)return;
      var angle=d.val/donutTotal*Math.PI*2;
      var x1=donutCx+donutR*Math.cos(cumAngle);var y1=donutCy+donutR*Math.sin(cumAngle);
      var x2=donutCx+donutR*Math.cos(cumAngle+angle);var y2=donutCy+donutR*Math.sin(cumAngle+angle);
      var xi1=donutCx+donutInner*Math.cos(cumAngle);var yi1=donutCy+donutInner*Math.sin(cumAngle);
      var xi2=donutCx+donutInner*Math.cos(cumAngle+angle);var yi2=donutCy+donutInner*Math.sin(cumAngle+angle);
      var lg=angle>Math.PI?1:0;
      donutPaths+='<path d="M'+xi1+' '+yi1+' L'+x1+' '+y1+' A'+donutR+' '+donutR+' 0 '+lg+' 1 '+x2+' '+y2+' L'+xi2+' '+yi2+' A'+donutInner+' '+donutInner+' 0 '+lg+' 0 '+xi1+' '+yi1+' Z" fill="'+d.color+'" opacity=".9"><title>'+d.label+' '+d.val+' ครั้ง</title></path>';
      cumAngle+=angle;
    });
    var donutSvg='<svg width="150" height="150" viewBox="0 0 150 150" xmlns="http://www.w3.org/2000/svg">'
      +donutPaths
      +'<text x="75" y="72" text-anchor="middle" fill="var(--tx)" font-size="18" font-weight="700" font-family="IBM Plex Mono,monospace">'+totalActive+'</text>'
      +'<text x="75" y="87" text-anchor="middle" fill="#8b949e" font-size="10">กำลังยืม</text>'
      +'</svg>';

    var legend=SET_DEFS.map(function(s){
      var pct=donutTotal>0?Math.round(setStats[s.id].totalLoans/donutTotal*100):0;
      return'<div style="display:flex;align-items:center;gap:6px;font-size:12px">'
        +'<span style="display:inline-block;width:10px;height:10px;border-radius:2px;background:'+s.color+';flex-shrink:0"></span>'
        +'<span style="color:var(--tx)">'+s.label+'</span>'
        +'<span style="color:var(--mu);margin-left:auto">'+setStats[s.id].totalLoans+' ครั้ง ('+pct+'%)</span>'
        +'</div>';
    }).join('');

    var machineGrids=SET_DEFS.map(function(s){
      var st=setStats[s.id];
      var allMachKeys=Object.keys(st.machines).sort(function(a,b){
        var na=parseInt(a,10),nb=parseInt(b,10);
        if(!isNaN(na)&&!isNaN(nb))return na-nb;
        return a.localeCompare(b);
      });
      var chips=allMachKeys.map(function(eqNo){
        var m=st.machines[eqNo];var isAct=m.active;
        return'<span title="'+(isAct?'กำลังยืม: '+m.dept:'ยืมแล้ว '+m.count+' ครั้ง')+'" style="display:inline-flex;align-items:center;gap:3px;font-size:10px;padding:2px 6px;border-radius:4px;font-family:var(--mo);background:'+(isAct?s.bg:'rgba(139,148,158,.1)')+';color:'+(isAct?s.color:'#8b949e')+';border:1px solid '+(isAct?s.color.replace(')',', .4)').replace('rgb','rgba'):'rgba(139,148,158,.25)')+';">'
          +eqNo+(isAct?'<span style="width:5px;height:5px;border-radius:50%;background:'+s.color+';flex-shrink:0"></span>':'')+'</span>';
      }).join('');
      if(!allMachKeys.length)chips='<span style="font-size:11px;color:var(--mu)">ยังไม่มีข้อมูล</span>';
      return'<div style="background:var(--bg);border:1px solid var(--bd);border-radius:7px;padding:10px 12px;border-left:3px solid '+s.color+'">'
        +'<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px">'
          +'<span style="font-size:12px;font-weight:700;color:'+s.color+'">'+s.label+'</span>'
          +'<span style="font-size:11px;color:var(--mu)">ยืม '+st.totalLoans+' ครั้ง | active '+st.activeNow+' เครื่อง</span>'
        +'</div>'
        +'<div style="display:flex;flex-wrap:wrap;gap:4px">'+chips+'</div>'
        +'</div>';
    }).join('');

    var unknownKeys=Object.keys(unknownMachines);
    var unknownHtml=unknownKeys.length
      ?'<div style="margin-top:10px;padding:8px 12px;background:rgba(239,68,68,.06);border:1px solid rgba(239,68,68,.2);border-radius:6px;font-size:11px;color:var(--mu)">⚠️ หมายเลขเครื่องที่ไม่อยู่ใน set ใด: '
        +unknownKeys.map(function(k){return'<span style="font-family:var(--mo);background:rgba(239,68,68,.1);padding:1px 5px;border-radius:3px;color:var(--er)">'+k+'</span>';}).join(' ')
        +'</div>'
      :'';

    // filter badge
    var filterLabel='';
    if(filterDateS||filterDateE)filterLabel='<span style="background:rgba(0,200,255,.1);border:1px solid rgba(0,200,255,.3);border-radius:10px;padding:1px 9px;font-size:11px;color:var(--ac);margin-left:8px">📅 '+(filterDateS||'...')+' → '+(filterDateE||'...')+'</span>';

    return{
      cards:cards,donutSvg:donutSvg,legend:legend,machineGrids:machineGrids,
      unknownHtml:unknownHtml,totalLoans:totalLoans,totalActive:totalActive,filterLabel:filterLabel
    };
  };

  // render HFNC section
  window._renderHfncSection = function() {
    var cont=document.getElementById('hfncSetCont');if(!cont)return;
    var ds=(document.getElementById('hfncDateS')||{}).value||'';
    var de=(document.getElementById('hfncDateE')||{}).value||'';
    var detailOpen=document.getElementById('hfncDetailPane')&&document.getElementById('hfncDetailPane').style.display!=='none';
    var r=window._buildHfncSetHtml(ds,de);
    cont.innerHTML=
      '<div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:14px">'+r.cards+'</div>'
      +'<div style="display:grid;grid-template-columns:160px 1fr;gap:16px;align-items:center;margin-bottom:4px">'
        +'<div style="text-align:center">'+r.donutSvg+'</div>'
        +'<div style="display:flex;flex-direction:column;gap:8px">'+r.legend
          +'<div style="margin-top:4px;padding-top:8px;border-top:1px solid var(--bd);font-size:11px;color:var(--mu)">'
            +'รวมทั้งหมด <strong style="color:var(--tx)">'+r.totalLoans+'</strong> ครั้ง | กำลังยืม <strong style="color:var(--ok)">'+r.totalActive+'</strong> เครื่อง'
            +(ds||de?'<br><span style="color:var(--mu)">ช่วงวันที่ยืม: '+(ds||'...')+' ถึง '+(de||'...')+'</span>':'')
          +'</div>'
        +'</div>'
      +'</div>'
      +'<div id="hfncDetailPane" style="display:'+(detailOpen?'block':'none')+';margin-top:12px">'
        +'<div style="font-size:11px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.5px;margin-bottom:8px">หมายเลขเครื่องแต่ละ set</div>'
        +'<div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">'+r.machineGrids+'</div>'
        +'<div style="margin-top:8px;font-size:11px;color:var(--mu)">'
          +'<span style="display:inline-flex;align-items:center;gap:4px;margin-right:12px"><span style="width:8px;height:8px;border-radius:50%;background:var(--ok);display:inline-block"></span>กำลังยืม (ACTIVE)</span>'
          +'<span>สีเทา = คืนแล้ว</span>'
        +'</div>'
        +r.unknownHtml
      +'</div>';
    // sync toggle button text
    var btn=document.getElementById('hfncDetailBtn');
    if(btn)btn.textContent=detailOpen?'▲ ซ่อนรายละเอียด':'▼ ดูรายละเอียด';
  };

  var _hfncSetHtml='<div class="card">'
    +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:10px">'
      +'<div class="ctitle" style="margin:0">💨 การใช้ HFNC set แยกตามยี่ห้อ/รุ่น'
        +'<span style="font-size:10px;color:var(--mu);font-weight:400;text-transform:none;letter-spacing:0;margin-left:8px">ไม่รวม HFNC transfer</span>'
      +'</div>'
      +'<button id="hfncDetailBtn" onclick="(function(){var p=document.getElementById(\'hfncDetailPane\');var b=document.getElementById(\'hfncDetailBtn\');if(p&&p.style.display===\'none\'){p.style.display=\'block\';b.textContent=\'▲ ซ่อนรายละเอียด\';}else if(p){p.style.display=\'none\';b.textContent=\'▼ ดูรายละเอียด\';}})();" class="btn bg sm" style="font-size:11px">▼ ดูรายละเอียด</button>'
    +'</div>'
    // ── filter bar ──
    +'<div style="display:flex;gap:8px;align-items:flex-end;flex-wrap:wrap;margin-bottom:14px;padding:10px 12px;background:var(--s2);border-radius:6px;border:1px solid var(--bd)">'
      +'<div style="display:flex;flex-direction:column;gap:3px">'
        +'<label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px">วันที่ยืม จาก</label>'
        +'<input type="date" id="hfncDateS" onchange="window._renderHfncSection&&window._renderHfncSection()" style="background:var(--bg);border:1px solid var(--bd);border-radius:5px;padding:5px 8px;color:var(--tx);font-size:12px;color-scheme:dark">'
      +'</div>'
      +'<div style="display:flex;flex-direction:column;gap:3px">'
        +'<label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px">วันที่ยืม ถึง</label>'
        +'<input type="date" id="hfncDateE" onchange="window._renderHfncSection&&window._renderHfncSection()" style="background:var(--bg);border:1px solid var(--bd);border-radius:5px;padding:5px 8px;color:var(--tx);font-size:12px;color-scheme:dark">'
      +'</div>'
      +'<button class="btn bg sm" onclick="var s=document.getElementById(\'hfncDateS\'),e=document.getElementById(\'hfncDateE\');if(s)s.value=\'\';if(e)e.value=\'\';window._renderHfncSection&&window._renderHfncSection();" style="font-size:11px">✕ ล้าง</button>'
      +'<span style="font-size:11px;color:var(--mu);align-self:center">กรองตามวันที่ยืมของแต่ละรายการ</span>'
    +'</div>'
    +'<div id="hfncSetCont"></div>'
    +'</div>';

  // trigger render after DOM ready
  setTimeout(function(){if(window._renderHfncSection)window._renderHfncSection();},50);

  sc.innerHTML=chartHtml+dayTableHtml+avgHtml+_hfncSetHtml+origHtml+allLoansHtml;

  if(sc._svh)sc.removeEventListener('click',sc._svh);
  sc._svh=function(e){
    var cirow=e.target.closest('.cirow');
    if(cirow&&cirow.dataset.lid){viewDet(cirow.dataset.lid);return;}
    // avg tile click
    var tile=e.target.closest('.avg-tile');
    if(tile&&tile.dataset.eq){sumFilterByEq(tile.dataset.eq);return;}
    // all loans table — view button
    var vbtn=e.target.closest('.alview');
    if(vbtn&&vbtn.dataset.lid){e.stopPropagation();viewDet(vbtn.dataset.lid);return;}
    // all loans table row click
    var alrow=e.target.closest('.alrow');
    if(alrow&&alrow.dataset.lid){viewDet(alrow.dataset.lid);return;}
    var btn=e.target.closest('.svb');if(!btn||!btn.dataset.key)return;
    var d=_sMap[btn.dataset.key];if(!d)return;
    if(d.type==='eq'){
      showPg('all',document.querySelectorAll('.nt')[4]);
      setTimeout(function(){var sel=document.getElementById('allEq');if(sel){sel.value=d.eq;renderAll();}},100);
    }else if(d.LoanID){viewDet(S(d.LoanID));}
  };
  sc.addEventListener('click',sc._svh);
  // avg tile hover via event delegation
  if(sc._moh)sc.removeEventListener('mouseover',sc._moh);
  if(sc._mol)sc.removeEventListener('mouseout',sc._mol);
  sc._moh=function(e){
    var t=e.target.closest('.avg-tile');if(t){t.style.boxShadow='0 0 0 2px rgba(0,200,255,.3)';t.style.transform='translateY(-2px)';}
    var r=e.target.closest('.alrow');if(r)r.style.background='rgba(0,200,255,.04)';
    var tr=e.target.closest('.avg-tbl-row');if(tr)tr.style.background='rgba(255,255,255,.02)';
  };
  sc._mol=function(e){
    var t=e.target.closest('.avg-tile');if(t){t.style.boxShadow='';t.style.transform='';}
    var r=e.target.closest('.alrow');if(r)r.style.background='';
    var tr=e.target.closest('.avg-tbl-row');if(tr)tr.style.background='';
  };
  sc.addEventListener('mouseover',sc._moh);
  sc.addEventListener('mouseout',sc._mol);

  // ── animate horizontal bars (requestAnimationFrame) ──
  setTimeout(function(){
    var cont=document.getElementById('alBarCont');
    if(!cont)return;
    var fills=cont.querySelectorAll('.al-bar-fill');
    // ต้องคำนวณ width จาก parent flex
    var rows2=cont.querySelectorAll('.al-bar-row');
    rows2.forEach(function(row,i){
      var fill=row.querySelector('.al-bar-fill');
      if(!fill)return;
      // ดึง pct จาก title
      var title=row.getAttribute('title')||'';
      var m=title.match(/: (\d+) รายการ/);
      if(!m)return;
      var val=parseInt(m[1]);
      // หา max จากทุก row
      var allVals=[];
      rows2.forEach(function(r){
        var t2=r.getAttribute('title')||'';
        var m2=t2.match(/: (\d+) รายการ/);
        if(m2)allVals.push(parseInt(m2[1]));
      });
      var mx=Math.max.apply(null,allVals)||1;
      var pct=val/mx*100;
      // get container width
      var contW=fill.parentElement?fill.parentElement.getBoundingClientRect().width-40:300;
      var targetW=Math.round(pct/100*contW);
      fill.style.width='0px';
      fill.style.transition='none';
      requestAnimationFrame(function(){
        requestAnimationFrame(function(){
          fill.style.transition='width .5s ease';
          fill.style.width=Math.max(4,targetW)+'px';
        });
      });
    });
  },80);

  // ── bar row click: filter ตาราง ──
  if(sc._barh)sc.removeEventListener('click',sc._barh);
  sc._barh=function(e){
    var row=e.target.closest('.al-bar-row');if(!row)return;
    var eq=row.dataset.eq;if(!eq)return;
    // highlight selected bar
    sc.querySelectorAll('.al-bar-row').forEach(function(r){r.style.background='';});
    row.style.background='rgba(0,200,255,.08)';
    // filter alTbody
    var tbody=document.getElementById('alTbody');if(!tbody)return;
    var rows=tbody.querySelectorAll('.alrow');
    var shown=0;
    rows.forEach(function(tr){
      var match=!eq||tr.dataset.eq===eq;
      tr.style.display=match?'':'none';
      if(match)shown++;
    });
    var cnt=document.getElementById('alCount');if(cnt)cnt.textContent=shown+' รายการ ('+eq+')';
    var clrBtn=document.getElementById('alClearEqFilter');if(clrBtn)clrBtn.style.display='inline-flex';
    // clear search
    var srch=document.getElementById('alSearch');if(srch)srch.value='';
  };
  sc.addEventListener('click',sc._barh);

  // ── bar row hover ──
  if(sc._barmoh)sc.removeEventListener('mouseover',sc._barmoh);
  if(sc._barmol)sc.removeEventListener('mouseout',sc._barmol);
  sc._barmoh=function(e){var r=e.target.closest('.al-bar-row');if(r)r.style.background='rgba(0,200,255,.06)';};
  sc._barmol=function(e){var r=e.target.closest('.al-bar-row');if(r&&r.style.background!=='rgba(0,200,255,.08)')r.style.background='';};
  sc.addEventListener('mouseover',sc._barmoh);
  sc.addEventListener('mouseout',sc._barmol);
}

function alClearEqFilter(){
  var tbody=document.getElementById('alTbody');
  if(tbody)tbody.querySelectorAll('.alrow').forEach(function(tr){tr.style.display='';});
  var cnt=document.getElementById('alCount');
  if(cnt&&tbody)cnt.textContent=tbody.querySelectorAll('.alrow').length+' รายการ';
  var btn=document.getElementById('alClearEqFilter');if(btn)btn.style.display='none';
  var sc=document.getElementById('sumCont');
  if(sc)sc.querySelectorAll('.al-bar-row').forEach(function(r){r.style.background='';});
  var srch=document.getElementById('alSearch');if(srch)srch.value='';
}

function sumFilterByEq(eqName){
  showPg('all',document.querySelectorAll('.nt')[4]);
  setTimeout(function(){var sel=document.getElementById('allEq');if(sel){sel.value=eqName;renderAll();}},100);
}
function sumFilterByDate(dateStr){
  showPg('all',document.querySelectorAll('.nt')[4]);
  setTimeout(function(){
    var ds=document.getElementById('allDateS');var de=document.getElementById('allDateE');
    if(ds)ds.value=dateStr;if(de)de.value=dateStr;renderAll();
  },100);
}
function filterAllByEqNo(eqNo){
  showPg('all',document.querySelectorAll('.nt')[4]);
  setTimeout(function(){
    var s=document.getElementById('allSearch');
    if(s){s.value=eqNo;renderAll();}
  },100);
}


function alFilterRows(q){
  var ql=(q||'').toLowerCase().trim();
  var tbody=document.getElementById('alTbody');if(!tbody)return;
  var rows=tbody.querySelectorAll('.alrow');
  var shown=0;
  rows.forEach(function(tr){
    var match=!ql||tr.dataset.search.includes(ql);
    tr.style.display=match?'':'none';
    if(match)shown++;
  });
  var cnt=document.getElementById('alCount');
  if(cnt)cnt.textContent=shown+' รายการ'+(ql?' (ค้นหา: "'+ql+'"）':'');
  // clear bar highlight when searching
  var sc=document.getElementById('sumCont');
  if(ql&&sc)sc.querySelectorAll('.al-bar-row').forEach(function(r){r.style.background='';});
  var btn=document.getElementById('alClearEqFilter');if(btn)btn.style.display=ql?'none':'none';
}
function filterEqDays(q){var ql=(q||'').toLowerCase().trim();var rows=document.querySelectorAll('#eqDaysTbody .eqdrow');rows.forEach(function(tr){var match=!ql||tr.dataset.eqno.includes(ql)||tr.dataset.eqname.includes(ql);tr.style.display=match?'':'none';});}
function filterAllByEq(eqName){showPg('all',document.querySelectorAll('.nt')[4]);setTimeout(function(){var sel=document.getElementById('allEq');if(sel){sel.value=eqName;renderAll();}},100);}
function filterAllByDate(dateStr){showPg('all',document.querySelectorAll('.nt')[4]);setTimeout(function(){var ds=document.getElementById('allDateS');var de=document.getElementById('allDateE');if(ds)ds.value=dateStr;if(de)de.value=dateStr;renderAll();},100);}

function viewDet(loanID){
  var lid=S(loanID);if(!lid||!loans)return;var loan=loans.find(function(l){return S(l.LoanID)===lid;});if(!loan)return;
  var bs=borrows?borrows.filter(function(b){return S(b.LoanID)===lid;}):[];var now=new Date(),ld=loan.LoanDate;var days=Math.max(0,Math.round((now-new Date(ld))/86400000));
  // คำนวณสถานะจริงจาก borrow records (ไม่ใช่จาก loan.Status ใน DB)
  var _effStatuses=(loan.EquipmentList||[]).map(function(e,idx){var b=bs.find(function(b){return N(b.EquipItemIndex)===idx;});return computeStatus(b);});
  var _hasPend=_effStatuses.indexOf('PENDING')>=0,_hasAct=_effStatuses.indexOf('ACTIVE')>=0,_hasAccP=_effStatuses.indexOf('ACC_PENDING')>=0;
  var _allRet=_effStatuses.length>0&&_effStatuses.every(function(s){return s==='RETURNED'||s==='ACC_PENDING';});
  var _effStatus=loan.Status==='CANCELLED'?'CANCELLED':_hasPend?'PENDING':_hasAct?'ACTIVE':_hasAccP?'ACC_PENDING':_allRet?'RETURNED':S(loan.Status);
  var html='<div style="display:grid;grid-template-columns:1fr 1fr;gap:9px;margin-bottom:11px"><div><span style="color:var(--mu);font-size:10px">LOAN ID</span><br><span style="font-family:var(--mo);color:var(--ac)">'+lid+'</span></div><div><span style="color:var(--mu);font-size:10px">สถานะ</span><br>'+badge(_effStatus)+'</div><div><span style="color:var(--mu);font-size:10px">วันที่ยืม</span><br>'+ld+'</div><div><span style="color:var(--mu);font-size:10px">จำนวนวัน</span><br><span style="font-family:var(--mo)">'+days+' วัน</span></div><div><span style="color:var(--mu);font-size:10px">ผู้ยืม</span><br>'+S(loan.BorrowerName)+'</div><div><span style="color:var(--mu);font-size:10px">แผนก</span><br>'+S(loan.DeptName)+'</div><div style="grid-column:1/-1"><span style="color:var(--mu);font-size:10px">ผู้ป่วย</span><br>'+S(loan.PatientName)+'</div></div><div style="background:var(--s2);border-radius:6px;padding:10px;margin-bottom:10px"><div style="font-size:10px;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;margin-bottom:6px">รายการเครื่องมือ ('+((loan.EquipmentList||[]).length)+' รายการ)</div>'+(loan.EquipmentList||[]).map(function(e,idx){var b=bs.find(function(b){return N(b.EquipItemIndex)===idx;});var itemStatus=computeStatus(b);var accHtml='';if(b&&b.Accessories&&b.Accessories.length){accHtml='<div style="margin-top:6px;padding:6px;background:var(--bg);border-radius:5px;font-size:12px">'+b.Accessories.map(function(a){var isRet=b.ReturnedAccs&&b.ReturnedAccs.some(function(r){return r.id===a.id;});var retAcc=isRet&&b.ReturnedAccs?b.ReturnedAccs.find(function(r){return r.id===a.id;}):null;var retAccDate=retAcc&&retAcc.date?retAcc.date:(isRet&&b.ReturnDate?b.ReturnDate:'');return'<div style="display:flex;justify-content:space-between;padding:3px 0;border-bottom:1px solid var(--bd)"><span>'+(a.name||'?')+(a.no?' No.'+a.no:'')+'</span>'+(isRet?'<span style="color:var(--ok);font-size:11px">✓ คืนแล้ว'+(retAccDate?' <span style="color:var(--mu);font-size:10px">('+retAccDate+')</span>':'')+'</span>':'<span style="color:var(--er);font-size:11px">⏳ ยังไม่คืน</span>')+'</div>';}).join('')+'</div>';}var thHtml='';if(b&&b.TransferHistory&&b.TransferHistory.length){var _th=b.TransferHistory;var _origin;if(_th[0]&&S(_th[0].fromDept)){_origin=S(_th[0].fromDept);}else if(_th[0]&&_th[0].note){var _nm=S(_th[0].note).match(/โอนจาก\s*(.+)/);_origin=_nm&&_nm[1].trim()?_nm[1].trim():S(loan.DeptName);}else{_origin=S(loan.DeptName);}var _bc=[_origin].concat(_th.map(function(t){return S(t.toDept);}));var _bcHtml=_bc.map(function(d,i){var isCur=i===_bc.length-1;if(isCur)return'<span style="background:rgba(0,200,255,.12);border:1px solid rgba(0,200,255,.4);border-radius:4px;padding:1px 8px;color:var(--ac);font-weight:700;font-size:11px">'+d+' <span style="font-size:9px;opacity:.8">ปัจจุบัน</span></span>';return'<span style="background:var(--s2);border:1px solid var(--bd);border-radius:4px;padding:1px 8px;color:var(--mu);font-size:11px">'+d+'</span> <span style="color:var(--mu);font-size:10px">›</span> ';}).join('');var _steps=[{dept:_origin,date:S(loan.LoanDate),who:S(loan.BorrowerName),isOrigin:true}];_th.forEach(function(t){_steps.push({dept:S(t.toDept),date:S(t.date),who:S(t.transfererName||''),isOrigin:false});});var _timelineHtml=_steps.map(function(step,si){var isLast=si===_steps.length-1;var dotStyle=isLast?'width:10px;height:10px;border-radius:50%;background:var(--ac);flex-shrink:0;margin-top:3px':'width:10px;height:10px;border-radius:50%;background:var(--s2);border:1.5px solid var(--bd);flex-shrink:0;margin-top:3px';var lineHtml=!isLast?'<div style="width:1px;flex:1;background:var(--bd);min-height:20px;margin:2px 0"></div>':'';var labelHtml=step.isOrigin?step.dept+' <span style="color:var(--mu);font-weight:400">(ต้นทาง)</span>':'โอน → '+step.dept+(isLast?' <span style="background:rgba(0,200,255,.15);color:var(--ac);font-size:10px;padding:0 6px;border-radius:99px;margin-left:4px">ปัจจุบัน</span>':'');return'<div style="display:flex;gap:10px;align-items:flex-start"><div style="display:flex;flex-direction:column;align-items:center;width:18px"><div style="'+dotStyle+'"></div>'+lineHtml+'</div><div style="padding-bottom:12px"><div style="font-size:12px;font-weight:700;color:var(--tx)">'+labelHtml+'</div><div style="font-size:11px;color:var(--mu);margin-top:2px">'+S(step.date)+(step.who?' · '+step.who:'')+'</div></div></div>';}).join('');thHtml='<div style="margin-top:8px;padding:10px 12px;background:rgba(0,200,255,.04);border:1px solid rgba(0,200,255,.12);border-radius:7px"><div style="font-size:10px;color:var(--ac);font-weight:700;text-transform:uppercase;letter-spacing:.5px;margin-bottom:8px">เส้นทางการโอน</div><div style="display:flex;align-items:center;flex-wrap:wrap;gap:5px;margin-bottom:10px;padding-bottom:10px;border-bottom:1px solid var(--bd)"><span style="font-size:11px;color:var(--mu)">เส้นทาง</span> '+_bcHtml+'</div>'+_timelineHtml+'</div>';}return'<div style="padding:5px 0;border-bottom:1px solid var(--bd)"><div style="display:flex;justify-content:space-between;align-items:center"><span><strong>#'+idx+'</strong> '+e.name+(e.detail?' — '+e.detail:'')+'</span><div>'+badge(itemStatus)+' <span style="font-family:var(--mo)">×'+e.qty+'</span></div></div>'+(b?'<div style="font-size:11px;color:var(--mu);padding-top:2px;padding-left:11px">เลขเครื่อง: <span style="color:var(--ac);font-family:var(--mo)">'+(S(b.EquipmentNo)||'—')+'</span> | ผู้ให้ยืม: '+S(b.GiverName||'-')+(S(b.ReceiverName)?' | ผู้รับคืน: '+S(b.ReceiverName):'')+(b.MachineReturnDate?' | <span style="color:var(--ok)">📅 คืนเครื่อง: '+S(b.MachineReturnDate)+'</span>':b.ReturnDate&&b.MachineReturned?' | <span style="color:var(--ok)">📅 คืนเครื่อง: '+S(b.ReturnDate)+'</span>':'')+'</div>':'')+accHtml+thHtml+'</div>';}).join('')+'</div>';
  document.getElementById('detBody').innerHTML=html;openM('mDetail');
}

// ════════════════════════════════════════════════════════════
// ADMIN FUNCTIONS
// ════════════════════════════════════════════════════════════

function checkAdminPin(){
  var inp=document.getElementById('adminPinInput');var err=document.getElementById('adminPinErr');
  if(!inp)return;
  if(inp.value===ADMIN_PIN){
    _adminUnlocked=true;
    document.getElementById('adminLock').style.display='none';
    document.getElementById('adminContent').style.display='block';
    adminRefresh();
  }else{
    err.textContent='รหัส Admin ไม่ถูกต้อง';inp.value='';inp.focus();
    setTimeout(function(){err.textContent='';},2500);
  }
}
document.addEventListener('DOMContentLoaded',function(){
  var inp=document.getElementById('adminPinInput');
  if(inp)inp.addEventListener('keydown',function(e){if(e.key==='Enter')checkAdminPin();});
});
function lockAdmin(){
  _adminUnlocked=false;
  document.getElementById('adminLock').style.display='flex';
  document.getElementById('adminContent').style.display='none';
  document.getElementById('adminPinInput').value='';
}
function adminRefresh(){
  if(!_adminUnlocked)return;
  var btn=document.getElementById('admRefreshBtn');
  if(btn){btn.disabled=true;btn.innerHTML='⏳ กำลังโหลด...';}
  Promise.all([API.getAllLoans(true),API.getAllBorrows(true)]).then(function(res){
    loans=res[0];borrows=res[1];
    renderAdminLoans();renderAdminBorrows();
    if(btn){btn.disabled=false;btn.innerHTML='🔄 Refresh';}
    toast('โหลดข้อมูลทั้งหมด '+loans.length+' ใบยืม / '+borrows.length+' บันทึก','success');
  }).catch(function(e){
    if(btn){btn.disabled=false;btn.innerHTML='🔄 Refresh';}
    toast('โหลดล้มเหลว: '+e.message,'error');
  });
}
function showAdminTab(id,el){
  document.querySelectorAll('.admin-sub').forEach(function(s){s.classList.remove('on');});
  document.querySelectorAll('.atab').forEach(function(t){t.classList.remove('on');});
  if(id==='pin-mgr'){
    var sa=document.getElementById('showAppPin');var sd=document.getElementById('showAdmPin');
    if(sa)sa.value=APP_PIN;if(sd)sd.value=ADMIN_PIN;
  }
  document.getElementById('adm-'+id).classList.add('on');
  if(el)el.classList.add('on');
  if(id==='loan-mgr')renderAdminLoans();
  else if(id==='borrow-mgr')renderAdminBorrows();
  else if(id==='lists-mgr')initListsTab();
}
function admClearFilter(type){
  if(type==='loan'){['admLoanSearch','admLoanDateS','admLoanDateE'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});document.getElementById('admLoanStat').value='';renderAdminLoans();}
  else{['admBorSearch'].forEach(function(id){var e=document.getElementById(id);if(e)e.value='';});document.getElementById('admBorStat').value='';document.getElementById('admBorEq').value='';renderAdminBorrows();}
}

// ── RENDER ADMIN LOAN TABLE ──
function renderAdminLoans(){
  if(!loans)return;
  var tbody=document.getElementById('admLoanBody');if(!tbody)return;
  var search=(document.getElementById('admLoanSearch')||{}).value||'';search=search.toLowerCase();
  var stat=(document.getElementById('admLoanStat')||{}).value||'';
  var ds=(document.getElementById('admLoanDateS')||{}).value||'';
  var de=(document.getElementById('admLoanDateE')||{}).value||'';
  var filtered=loans.filter(function(l){
    if(stat&&S(l.Status)!==stat)return false;
    var ld=l.LoanDate;if(ds&&ld&&ld<ds)return false;if(de&&ld&&ld>de)return false;
    var txt=S(l.LoanID)+' '+S(l.BorrowerName)+' '+S(l.DeptName)+' '+S(l.PatientName);
    return!search||txt.toLowerCase().includes(search);
  });
  document.getElementById('admLoanCount').textContent='แสดง '+filtered.length+' จาก '+loans.length+' รายการ';
  if(!filtered.length){tbody.innerHTML='<tr><td colspan="9" style="text-align:center;color:var(--mu);padding:20px">ไม่มีข้อมูล</td></tr>';return;}
  var smap={PENDING:'bp2',ACTIVE:'ba',RETURNED:'br2',CANCELLED:'bc2'};
  tbody.innerHTML=filtered.map(function(l){
    var eqNames=(l.EquipmentList||[]).map(function(e){return e.name;}).join(', ');
    return'<tr class="edit-row">'
      +'<td><input type="checkbox" class="bulk-sel loan-chk" value="'+S(l.LoanID)+'" onchange="updateBulkBar(\'loan\')"></td>'
      +'<td style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+S(l.LoanID)+'</td>'
      +'<td>'+S(l.LoanDate)+'</td>'
      +'<td>'+S(l.BorrowerName)+'</td>'
      +'<td>'+S(l.DeptName)+'</td>'
      +'<td>'+S(l.PatientName)+'</td>'
      +'<td style="font-size:11px;color:var(--mu)">'+eqNames+'</td>'
      +'<td><span class="badge '+(smap[S(l.Status)]||'bp2')+'">'+S(l.Status)+'</span></td>'
      +'<td><div style="display:flex;gap:3px;flex-wrap:wrap">'
        +'<button class="abt" style="border-color:#f59e0b;color:#f59e0b;font-size:11px" onclick="openEditLoan(\''+S(l.LoanID)+'\')">✏️ แก้ไข</button>'
        +'<button class="abt" style="border-color:var(--ac);color:var(--ac);font-size:11px" onclick="viewDet(\''+S(l.LoanID)+'\')">👁 ดู</button>'
        +'<button class="abt" style="border-color:var(--er);color:var(--er);font-size:11px" onclick="admDeleteLoan(\''+S(l.LoanID)+'\')">🗑</button>'
      +'</div></td></tr>';
  }).join('');
}

// ── RENDER ADMIN BORROW TABLE ──
function renderAdminBorrows(){
  if(!borrows)return;
  var tbody=document.getElementById('admBorBody');if(!tbody)return;
  var search=(document.getElementById('admBorSearch')||{}).value||'';search=search.toLowerCase();
  var stat=(document.getElementById('admBorStat')||{}).value||'';
  var eq=(document.getElementById('admBorEq')||{}).value||'';
  var filtered=borrows.filter(function(b){
    if(stat&&S(b.Status)!==stat)return false;
    if(eq){var loanObj=loans?loans.find(function(l){return S(l.LoanID)===S(b.LoanID);}):null;var eqName=loanObj&&loanObj.EquipmentList&&loanObj.EquipmentList[b.EquipItemIndex]?loanObj.EquipmentList[b.EquipItemIndex].name:'';if(S(eqName)!==eq)return false;}
    var txt=S(b.RecordID)+' '+S(b.LoanID)+' '+S(b.EquipmentNo)+' '+S(b.GiverName)+' '+S(b.ReceiverName);
    return!search||txt.toLowerCase().includes(search);
  });
  document.getElementById('admBorCount').textContent='แสดง '+filtered.length+' จาก '+borrows.length+' รายการ';
  if(!filtered.length){tbody.innerHTML='<tr><td colspan="10" style="text-align:center;color:var(--mu);padding:20px">ไม่มีข้อมูล</td></tr>';return;}
  var smap={ACTIVE:'ba',RETURNED:'br2',ACC_PENDING:'bap'};
  var getLoanEqName=function(b){var loanObj=loans?loans.find(function(l){return S(l.LoanID)===S(b.LoanID);}):null;return loanObj&&loanObj.EquipmentList&&loanObj.EquipmentList[b.EquipItemIndex]?loanObj.EquipmentList[b.EquipItemIndex].name:'?';};
  tbody.innerHTML=filtered.map(function(b){
    return'<tr class="edit-row">'
      +'<td><input type="checkbox" class="bulk-sel bor-chk" value="'+S(b.RecordID)+'" onchange="updateBulkBar(\'borrow\')"></td>'
      +'<td style="font-family:var(--mo);font-size:10px;color:var(--ac2)">'+S(b.RecordID)+'</td>'
      +'<td style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+S(b.LoanID)+'</td>'
      +'<td>'+S(b.GiverName||'-')+'</td>'
      +'<td style="font-family:var(--mo);color:var(--ac);font-weight:700">'+S(b.EquipmentNo||'—')+'</td>'
      +'<td style="font-size:11px">'+getLoanEqName(b)+'</td>'
      +'<td>'+S(b.ReturnDate||'-')+'</td>'
      +'<td>'+S(b.ReceiverName||'-')+'</td>'
      +'<td><span class="badge '+(smap[S(b.Status)]||'bp2')+'">'+S(b.Status)+'</span></td>'
      +'<td><div style="display:flex;gap:3px;flex-wrap:wrap">'
        +'<button class="abt" style="border-color:#f59e0b;color:#f59e0b;font-size:11px" onclick="openEditBorrow(\''+S(b.RecordID)+'\')">✏️ แก้ไข</button>'
        +'<button class="abt" style="border-color:var(--er);color:var(--er);font-size:11px" onclick="admDeleteBorrow(\''+S(b.RecordID)+'\')">🗑</button>'
      +'</div></td></tr>';
  }).join('');
}

// ── OPEN EDIT LOAN MODAL ──
function openEditLoan(loanID){
  var l=loans?loans.find(function(x){return S(x.LoanID)===S(loanID);}):null;if(!l)return;
  document.getElementById('admEditLoanID').value=S(l.LoanID);
  document.getElementById('admEditLoanIDDisp').value=S(l.LoanID);
  document.getElementById('admEditLoanDate').value=S(l.LoanDate);
  document.getElementById('admEditBorrower').value=S(l.BorrowerName);
  document.getElementById('admEditDept').value=S(l.DeptName);
  document.getElementById('admEditDeptCode').value=S(l.DeptCode||'9999');
  document.getElementById('admEditPatient').value=S(l.PatientName);
  document.getElementById('admEditStatus').value=S(l.Status);
  document.getElementById('admEditEqList').value=JSON.stringify(l.EquipmentList||[],null,2);
  openM('mAdmEditLoan');
}
async function admSaveEditLoan(){
  var loanID=S(document.getElementById('admEditLoanID').value);if(!loanID)return;
  var eqListRaw=document.getElementById('admEditEqList').value;
  var eqList;
  try{eqList=JSON.parse(eqListRaw);}catch(e){bigToast('รายการเครื่องมือ JSON ไม่ถูกต้อง\n'+e.message,'error');return;}
  try{
    await API.updateLoan(loanID,{
      loanDate:document.getElementById('admEditLoanDate').value,
      borrowerName:document.getElementById('admEditBorrower').value.trim(),
      deptName:document.getElementById('admEditDept').value.trim(),
      deptCode:document.getElementById('admEditDeptCode').value.trim(),
      patientName:document.getElementById('admEditPatient').value.trim(),
      status:document.getElementById('admEditStatus').value,
      equipmentList:eqList
    });
    bigToast('บันทึกการแก้ไขสำเร็จ\n'+loanID,'success');closeM('mAdmEditLoan');setTimeout(function(){loadAll().then(renderAdminLoans);},1500);
  }catch(e){bigToast('บันทึกล้มเหลว\n'+e.message,'error');}
}
async function admDeleteLoan(loanID){
  if(!loanID){bigToast('ไม่พบ LoanID','error');return;}
  if(!confirm('⚠️ ลบใบยืม '+loanID+' และบันทึกการยืมทั้งหมดที่เกี่ยวข้อง?\n(ไม่สามารถกู้คืนได้)'))return;
  try{await API.deleteLoan(loanID);bigToast('ลบสำเร็จ\n'+loanID,'success');closeM('mAdmEditLoan');setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('ลบล้มเหลว\n'+e.message,'error');}
}

// ── OPEN EDIT BORROW MODAL ──
function openEditBorrow(recID){
  var b=borrows?borrows.find(function(x){return S(x.RecordID)===S(recID);}):null;if(!b)return;
  document.getElementById('admEditRecID').value=S(b.RecordID);
  document.getElementById('admEditRecIDDisp').value=S(b.RecordID);
  document.getElementById('admEditRecLoanID').value=S(b.LoanID);
  document.getElementById('admEditGiver').value=S(b.GiverName||'');
  document.getElementById('admEditEqNo').value=S(b.EquipmentNo||'');
  document.getElementById('admEditRetDate').value=S(b.ReturnDate||'');
  document.getElementById('admEditReceiver').value=S(b.ReceiverName||'');
  document.getElementById('admEditRetRemark').value=S(b.ReturnRemark||'');
  document.getElementById('admEditRecStatus').value=S(b.Status);
  document.getElementById('admEditMachineRet').value=b.MachineReturned?'true':'false';
  openM('mAdmEditBorrow');
}
async function admSaveEditBorrow(){
  var recID=S(document.getElementById('admEditRecID').value);if(!recID)return;
  var body={
    loan_id:document.getElementById('admEditRecLoanID').value.trim(),
    giver_name:document.getElementById('admEditGiver').value.trim(),
    equipment_no:document.getElementById('admEditEqNo').value.trim(),
    return_date:document.getElementById('admEditRetDate').value||null,
    receiver_name:document.getElementById('admEditReceiver').value.trim(),
    return_remark:document.getElementById('admEditRetRemark').value.trim(),
    status:document.getElementById('admEditRecStatus').value,
    machine_returned:document.getElementById('admEditMachineRet').value==='true'
  };
  try{
    await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(recID),body);
    bigToast('บันทึกการแก้ไขสำเร็จ\n'+recID,'success');closeM('mAdmEditBorrow');setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);
  }catch(e){bigToast('บันทึกล้มเหลว\n'+e.message,'error');}
}
async function admDeleteBorrow(recID){
  if(!recID){bigToast('ไม่พบ RecordID','error');return;}
  if(!confirm('⚠️ ลบบันทึกการยืม '+recID+'?\n(ไม่สามารถกู้คืนได้)'))return;
  try{await SB.del('borrow_records','record_id=eq.'+encodeURIComponent(recID));bigToast('ลบสำเร็จ\n'+recID,'success');closeM('mAdmEditBorrow');setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);}
  catch(e){bigToast('ลบล้มเหลว\n'+e.message,'error');}
}

// ── ADD LOAN MODAL ──
function openAddLoanModal(){openM('mAddLoan');}
async function admAddLoan(){
  var date=document.getElementById('addLoanDate').value;
  var borrower=document.getElementById('addLoanBorrower').value.trim();
  var dept=document.getElementById('addLoanDept').value.trim();
  var deptCode=document.getElementById('addLoanDeptCode').value.trim()||'9999';
  var patient=document.getElementById('addLoanPatient').value.trim();
  var eq=document.getElementById('addLoanEq').value;
  var detail=document.getElementById('addLoanDetail').value.trim();
  var status=document.getElementById('addLoanStatus').value;
  if(!date||!borrower||!dept||!patient||!eq){bigToast('กรุณากรอกข้อมูลให้ครบ','error');return;}
  var ts=new Date();
  var id='LN-'+ts.getFullYear()+String(ts.getMonth()+1).padStart(2,'0')+String(ts.getDate()).padStart(2,'0')+'-'+Math.random().toString(36).substr(2,8).toUpperCase();
  try{
    await SB.post('loan_requests',{loan_id:id,loan_date:date,borrower_name:borrower,dept_code:deptCode,dept_name:dept,patient_name:patient,equipment_list:[{name:eq,qty:1,detail:detail}],status:status});
    bigToast('เพิ่มใบยืมสำเร็จ!\n'+id,'success');closeM('mAddLoan');setTimeout(function(){loadAll().then(renderAdminLoans);},1500);
  }catch(e){bigToast('เพิ่มล้มเหลว\n'+e.message,'error');}
}

// ── QUICK EDIT ──
function qeSearchLoan(){
  var q=(document.getElementById('qeLoanSearch').value||'').toLowerCase().trim();
  var res=document.getElementById('qeLoanResult');if(!q||!loans){res.innerHTML='';return;}
  var found=loans.filter(function(l){return(S(l.LoanID)+' '+S(l.BorrowerName)+' '+S(l.PatientName)).toLowerCase().includes(q);}).slice(0,5);
  if(!found.length){res.innerHTML='<div style="color:var(--mu);font-size:12px;padding:8px">ไม่พบ</div>';return;}
  res.innerHTML='<div style="display:flex;flex-direction:column;gap:4px;margin-top:6px">'+found.map(function(l){return'<div style="background:var(--s2);border-radius:5px;padding:8px 10px;display:flex;justify-content:space-between;align-items:center"><div><span style="font-family:var(--mo);font-size:11px;color:var(--ac)">'+S(l.LoanID)+'</span> <span style="font-size:12px">'+S(l.BorrowerName)+' / '+S(l.DeptName)+'</span><br><span style="font-size:11px;color:var(--mu)">ผู้ป่วย: '+S(l.PatientName)+' | '+S(l.LoanDate)+'</span></div><button class="btn badm sm" onclick="openEditLoan(\''+S(l.LoanID)+'\')">✏️</button></div>';}).join('')+'</div>';
}
function qeSearchBorrow(){
  var q=(document.getElementById('qeBorSearch').value||'').toLowerCase().trim();
  var res=document.getElementById('qeBorResult');if(!q||!borrows){res.innerHTML='';return;}
  var found=borrows.filter(function(b){return(S(b.RecordID)+' '+S(b.EquipmentNo)+' '+S(b.LoanID)).toLowerCase().includes(q);}).slice(0,5);
  if(!found.length){res.innerHTML='<div style="color:var(--mu);font-size:12px;padding:8px">ไม่พบ</div>';return;}
  res.innerHTML='<div style="display:flex;flex-direction:column;gap:4px;margin-top:6px">'+found.map(function(b){return'<div style="background:var(--s2);border-radius:5px;padding:8px 10px;display:flex;justify-content:space-between;align-items:center"><div><span style="font-family:var(--mo);font-size:11px;color:var(--ac2)">'+S(b.RecordID)+'</span><br><span style="font-size:12px">หมายเลขเครื่อง: <strong style="color:var(--ac)">'+S(b.EquipmentNo||'—')+'</strong> | LoanID: '+S(b.LoanID)+'</span><br><span style="font-size:11px;color:var(--mu)">ผู้ให้ยืม: '+S(b.GiverName||'-')+' | สถานะ: '+S(b.Status)+'</span></div><button class="btn badm sm" onclick="openEditBorrow(\''+S(b.RecordID)+'\')">✏️</button></div>';}).join('')+'</div>';
}
async function quickChangeStatus(){
  var lid=document.getElementById('qsLoanID').value.trim();var newStat=document.getElementById('qsNewStat').value;
  if(!lid){bigToast('กรุณาใส่ LoanID','error');return;}
  if(!confirm('เปลี่ยนสถานะ '+lid+' เป็น '+newStat+' ?'))return;
  try{await API.updateLoan(lid,{status:newStat});bigToast('เปลี่ยนสถานะสำเร็จ\n'+lid+' → '+newStat,'success');document.getElementById('qsLoanID').value='';setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function quickChangeDate(){
  var lid=document.getElementById('qdLoanID').value.trim();var newDate=document.getElementById('qdNewDate').value;
  if(!lid||!newDate){bigToast('กรุณากรอก LoanID และวันที่','error');return;}
  if(!confirm('แก้ไขวันที่ยืม '+lid+' เป็น '+newDate+' ?'))return;
  try{await API.updateLoan(lid,{loanDate:newDate});bigToast('แก้ไขวันที่สำเร็จ\n'+lid,'success');document.getElementById('qdLoanID').value='';setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function quickChangeFields(){
  var lid=document.getElementById('qfLoanID').value.trim();var dept=document.getElementById('qfNewDept').value.trim();var borrower=document.getElementById('qfNewBorrower').value.trim();var patient=document.getElementById('qfNewPatient').value.trim();
  if(!lid){bigToast('กรุณาใส่ LoanID','error');return;}
  var updates={};if(dept)updates.deptName=dept;if(borrower)updates.borrowerName=borrower;if(patient)updates.patientName=patient;
  if(!Object.keys(updates).length){bigToast('กรุณากรอกข้อมูลที่ต้องการแก้ไข','warning');return;}
  if(!confirm('แก้ไขข้อมูล '+lid+' ?'))return;
  try{await API.updateLoan(lid,updates);bigToast('บันทึกสำเร็จ\n'+lid,'success');['qfLoanID','qfNewDept','qfNewBorrower','qfNewPatient'].forEach(function(id){document.getElementById(id).value='';});setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function quickChangeEqNo(){
  var recID=document.getElementById('qeRecID').value.trim();var eqNo=document.getElementById('qeNewEqNo').value.trim();var giver=document.getElementById('qeNewGiver').value.trim();
  if(!recID){bigToast('กรุณาใส่ RecordID','error');return;}
  var body={};if(eqNo)body.equipment_no=eqNo;if(giver)body.giver_name=giver;
  if(!Object.keys(body).length){bigToast('กรุณากรอกข้อมูลที่ต้องการแก้ไข','warning');return;}
  if(!confirm('แก้ไขหมายเลขเครื่อง '+recID+' ?'))return;
  try{await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(recID),body);bigToast('บันทึกสำเร็จ\n'+recID,'success');['qeRecID','qeNewEqNo','qeNewGiver'].forEach(function(id){document.getElementById(id).value='';});setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function quickChangeReturn(){
  var recID=document.getElementById('qrRecID').value.trim();var retDate=document.getElementById('qrNewRetDate').value;var receiver=document.getElementById('qrNewReceiver').value.trim();var remark=document.getElementById('qrNewRemark').value.trim();
  if(!recID){bigToast('กรุณาใส่ RecordID','error');return;}
  var body={};if(retDate)body.return_date=retDate;if(receiver)body.receiver_name=receiver;if(remark)body.return_remark=remark;
  if(!Object.keys(body).length){bigToast('กรุณากรอกข้อมูลที่ต้องการแก้ไข','warning');return;}
  if(!confirm('แก้ไขข้อมูลการคืน '+recID+' ?'))return;
  try{await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(recID),body);bigToast('บันทึกสำเร็จ\n'+recID,'success');['qrRecID','qrNewRetDate','qrNewReceiver','qrNewRemark'].forEach(function(id){document.getElementById(id).value='';});setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}

// ── BULK SELECTION ──
function toggleSelectAll(type,checked){
  var cls=type==='loan'?'.loan-chk':'.bor-chk';
  document.querySelectorAll(cls).forEach(function(cb){cb.checked=checked;});
  updateBulkBar(type);
}
function updateBulkBar(type){
  if(type==='loan'){
    var sel=document.querySelectorAll('.loan-chk:checked');
    var bar=document.getElementById('admLoanBulkBar');
    bar.style.display=sel.length?'flex':'none';
    document.getElementById('loanSelCount').textContent=sel.length;
  }else{
    var sel2=document.querySelectorAll('.bor-chk:checked');
    var bar2=document.getElementById('admBorBulkBar');
    bar2.style.display=sel2.length?'flex':'none';
    document.getElementById('borSelCount').textContent=sel2.length;
  }
}
async function bulkChangeLoanStatus(newStatus){
  var sel=Array.from(document.querySelectorAll('.loan-chk:checked')).map(function(cb){return cb.value;});
  if(!sel.length){toast('ไม่มีรายการที่เลือก','warning');return;}
  if(!confirm('เปลี่ยนสถานะ '+sel.length+' รายการ เป็น '+newStatus+' ?'))return;
  try{for(var i=0;i<sel.length;i++)await API.updateLoan(sel[i],{status:newStatus});bigToast('เปลี่ยนสถานะสำเร็จ '+sel.length+' รายการ','success');setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function bulkDeleteLoans(){
  var sel=Array.from(document.querySelectorAll('.loan-chk:checked')).map(function(cb){return cb.value;});
  if(!sel.length){toast('ไม่มีรายการที่เลือก','warning');return;}
  if(!confirm('⚠️ ลบใบยืม '+sel.length+' รายการ?\n(ลบบันทึกการยืมที่เกี่ยวข้องด้วย — ไม่สามารถกู้คืนได้)'))return;
  try{for(var i=0;i<sel.length;i++)await API.deleteLoan(sel[i]);bigToast('ลบสำเร็จ '+sel.length+' รายการ','success');setTimeout(function(){loadAll().then(renderAdminLoans);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function bulkChangeBorStatus(newStatus){
  var sel=Array.from(document.querySelectorAll('.bor-chk:checked')).map(function(cb){return cb.value;});
  if(!sel.length){toast('ไม่มีรายการที่เลือก','warning');return;}
  if(!confirm('เปลี่ยนสถานะ '+sel.length+' รายการ เป็น '+newStatus+' ?'))return;
  try{for(var i=0;i<sel.length;i++)await SB.patch('borrow_records','record_id=eq.'+encodeURIComponent(sel[i]),{status:newStatus});bigToast('เปลี่ยนสถานะสำเร็จ '+sel.length+' รายการ','success');setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
async function bulkDeleteBorrows(){
  var sel=Array.from(document.querySelectorAll('.bor-chk:checked')).map(function(cb){return cb.value;});
  if(!sel.length){toast('ไม่มีรายการที่เลือก','warning');return;}
  if(!confirm('⚠️ ลบบันทึกการยืม '+sel.length+' รายการ?\n(ไม่สามารถกู้คืนได้)'))return;
  try{for(var i=0;i<sel.length;i++)await SB.del('borrow_records','record_id=eq.'+encodeURIComponent(sel[i]));bigToast('ลบสำเร็จ '+sel.length+' รายการ','success');setTimeout(function(){loadAll().then(renderAdminBorrows);},1500);}
  catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}

// ── BULK OPS ──
var _bulkDelList=[];
async function bulkDeleteByStatus(){
  var stat=document.getElementById('bulkDelStat').value;var before=document.getElementById('bulkDelBefore').value;
  if(!stat){toast('กรุณาเลือกสถานะ','warning');return;}
  if(!before){bigToast('กรุณาระบุวันที่ก่อน (ลบรายการที่เก่ากว่าวันที่นี้)','warning');return;}
  var filtered=(loans||[]).filter(function(l){return S(l.Status)===stat&&l.LoanDate&&l.LoanDate<before;});
  if(!filtered.length){bigToast('ไม่พบรายการที่ตรงเงื่อนไข','warning');return;}
  _bulkDelList=filtered.map(function(l){return l.LoanID;});
  var area=document.getElementById('bulkPreviewArea');var content=document.getElementById('bulkPreviewContent');
  content.innerHTML='<div style="margin-bottom:8px;font-size:13px;color:var(--wn)">⚠️ จะลบ <strong>'+_bulkDelList.length+'</strong> รายการที่มีสถานะ <strong>'+stat+'</strong> ก่อนวันที่ <strong>'+before+'</strong></div>'
    +'<div class="dtw"><table class="dt" style="font-size:12px"><thead><tr><th>LoanID</th><th>วันที่ยืม</th><th>ผู้ยืม</th><th>แผนก</th><th>สถานะ</th></tr></thead><tbody>'
    +filtered.slice(0,50).map(function(l){return'<tr><td style="font-family:var(--mo);font-size:10px">'+S(l.LoanID)+'</td><td>'+S(l.LoanDate)+'</td><td>'+S(l.BorrowerName)+'</td><td>'+S(l.DeptName)+'</td><td>'+badge(S(l.Status))+'</td></tr>';}).join('')
    +(filtered.length>50?'<tr><td colspan="5" style="text-align:center;color:var(--mu)">... และอีก '+(filtered.length-50)+' รายการ</td></tr>':'')
    +'</tbody></table></div>';
  area.style.display='block';
}
async function executeBulkDelete(){
  if(!_bulkDelList.length)return;
  if(!confirm('⚠️ ยืนยันการลบ '+_bulkDelList.length+' รายการ?\n(ไม่สามารถกู้คืนได้!!)'))return;
  try{
    var done=0;
    for(var i=0;i<_bulkDelList.length;i++){await API.deleteLoan(_bulkDelList[i]);done++;}
    bigToast('ลบสำเร็จ '+done+' รายการ','success');cancelBulkPreview();setTimeout(function(){loadAll().then(renderAdminLoans);},2000);
  }catch(e){bigToast('เกิดข้อผิดพลาด\n'+e.message,'error');}
}
function cancelBulkPreview(){_bulkDelList=[];document.getElementById('bulkPreviewArea').style.display='none';}

async function findOrphanRecords(){
  var res=document.getElementById('orphanResult');res.innerHTML='<div class="lding"><div class="spin"></div></div>';
  if(!loans||!borrows){res.innerHTML='<span style="color:var(--mu)">กรุณาโหลดข้อมูลก่อน</span>';return;}
  var loanIDs=new Set(loans.map(function(l){return l.LoanID;}));var orphans=borrows.filter(function(b){return!loanIDs.has(b.LoanID);});
  if(!orphans.length){res.innerHTML='<span style="color:var(--ok)">✅ ไม่พบ Orphan Records</span>';}
  else{res.innerHTML='<span style="color:var(--wn)">⚠️ พบ '+orphans.length+' Orphan Records: </span>'+orphans.map(function(b){return'<span style="font-family:var(--mo);font-size:10px;background:rgba(239,68,68,.1);padding:1px 5px;border-radius:3px;margin:2px;display:inline-block">'+S(b.RecordID)+'</span>';}).join('');}
}

async function recalcAllStatuses(){
  var res=document.getElementById('recalcResult');res.innerHTML='<div class="lding"><div class="spin"></div></div>';
  if(!loans||!borrows){res.innerHTML='<span style="color:var(--mu)">กรุณาโหลดข้อมูลก่อน</span>';return;}
  var updated=0,errors=0;
  var exp=expanded();var loanStatusMap={};
  exp.forEach(function(item){var lid=S(item.loan.LoanID);if(!loanStatusMap[lid])loanStatusMap[lid]=[];loanStatusMap[lid].push(item.status);});
  for(var lid in loanStatusMap){
    var statuses=loanStatusMap[lid];var hasPending=statuses.indexOf('PENDING')>=0,hasActive=statuses.indexOf('ACTIVE')>=0,hasAccP=statuses.indexOf('ACC_PENDING')>=0;var allRet=statuses.every(function(s){return s==='RETURNED'||s==='ACC_PENDING';});var allCancelled=statuses.every(function(s){return s==='CANCELLED';});
    var newStatus=allCancelled?'CANCELLED':hasPending?'PENDING':hasActive?'ACTIVE':hasAccP?'ACC_PENDING':allRet?'RETURNED':'PENDING';
    var current=loans.find(function(l){return l.LoanID===lid;});if(current&&S(current.Status)!==newStatus){try{await API.updateLoan(lid,{status:newStatus});updated++;}catch(e){errors++;}}
  }
  res.innerHTML='<span style="color:var(--ok)">✅ คำนวณสถานะเสร็จ — อัปเดต '+updated+' รายการ'+(errors?' | ❌ ล้มเหลว '+errors+' รายการ':'')+'</span>';
  if(updated)setTimeout(function(){loadAll().then(renderAdminLoans);},1500);
}

// ════════════════════════════════════════════════════════════
// LIST MANAGER FUNCTIONS
// ════════════════════════════════════════════════════════════

function showListTab(name,el){
  ['dept','staff','eq','acc','eqnum','retrem','hfncset'].forEach(function(t){
    var s=document.getElementById('lsec-'+t);if(s)s.style.display='none';
    var b=document.getElementById('ltab-'+t);if(b)b.classList.remove('on');
  });
  var sec=document.getElementById('lsec-'+name);if(sec)sec.style.display='block';
  if(el)el.classList.add('on');
  if(name==='dept'){_wDepts=DEPTS.map(function(d){return{c:d.c,n:d.n};});renderDeptList();}
  else if(name==='staff'){_wStaff=STAFF.slice();renderStaffList();}
  else if(name==='eq'){_wEql=EQL.slice();renderEqList();}
  else if(name==='acc'){_wAcc=ACCS.map(function(a){return{id:a.id,n:a.n,no:!!a.no,det:!!a.det};});renderAccList();}
  else if(name==='eqnum'){_wEqnum=EQNUMS.slice();_selEqnumIdx=-1;renderEqnumList();}
  else if(name==='retrem'){
    _wRetrem=RETREMARKS.map(function(r){
      return{label:r,needDetail:['ย้ายไปหน่วยวิกฤต','เครื่องมืออุปกรณ์ชำรุดขัดข้อง','อื่นๆ'].includes(r)};
    });
    _selRetremIdx=-1;renderRetremList();
  }
  else if(name==='hfncset'){renderHfncSetAdmin();}
}

// ── called when entering lists-mgr tab ──
function initListsTab(){
  _wDepts=DEPTS.map(function(d){return{c:d.c,n:d.n};});
  _wStaff=STAFF.slice();
  _wEql=EQL.slice();
  _wAcc=ACCS.map(function(a){return{id:a.id,n:a.n,no:!!a.no,det:!!a.det};});
  _wEqnum=EQNUMS.slice();
  _wRetrem=RETREMARKS.map(function(r){
    return{label:r,needDetail:['ย้ายไปหน่วยวิกฤต','เครื่องมืออุปกรณ์ชำรุดขัดข้อง','อื่นๆ'].includes(r)};
  });
  showListTab('dept',document.getElementById('ltab-dept'));
}

// ─────────────────── DEPT ───────────────────
function renderDeptList(){
  if(!_wDepts)return;
  var q=(document.getElementById('deptFilterInput').value||'').toLowerCase();
  var area=document.getElementById('deptListArea');
  var badge=document.getElementById('deptCountBadge');if(badge)badge.textContent=_wDepts.length;
  var html=_wDepts.map(function(d,i){
    if(q&&!d.n.toLowerCase().includes(q)&&!d.c.includes(q))return'';
    var sel=_selDeptIdx===i;
    return'<div onclick="selectDeptItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(245,158,11,.12)':'transparent')+';transition:background .1s" onmouseover="this.style.background=\'rgba(255,255,255,.04)\'" onmouseout="this.style.background=\''+(sel?'rgba(245,158,11,.12)':'transparent')+'\'"> '
      +'<span style="font-family:var(--mo);font-size:10px;color:var(--mu);width:36px;flex-shrink:0">'+d.c+'</span>'
      +'<span style="flex:1;font-size:13px"'+(sel?' style="font-weight:700;color:#f59e0b"':'')+'>'+d.n+'</span>'
      +'<div style="display:flex;gap:4px">'
        +'<button class="abt" style="font-size:10px;border-color:#f59e0b;color:#f59e0b" onclick="event.stopPropagation();startEditDept('+i+')">✏️</button>'
        +'<button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeDeptAt('+i+')">✕</button>'
      +'</div></div>';
  }).join('');
  area.innerHTML=html||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectDeptItem(i){_selDeptIdx=i;renderDeptList();}
function startEditDept(i){
  _selDeptIdx=i;
  document.getElementById('editDeptCode').value=_wDepts[i].c;
  document.getElementById('editDeptName').value=_wDepts[i].n;
  document.getElementById('editDeptPanel').style.display='block';
  renderDeptList();
}
function saveEditDept(){
  if(_selDeptIdx<0)return;
  var code=document.getElementById('editDeptCode').value.trim()||_wDepts[_selDeptIdx].c;
  var name=document.getElementById('editDeptName').value.trim();
  if(!name){toast('กรุณาระบุชื่อแผนก','error');return;}
  _wDepts[_selDeptIdx]={c:code,n:name};
  cancelEditDept();renderDeptList();
  toast('แก้ไขแผนกแล้ว (ยังไม่บันทึก กด "บันทึกการเปลี่ยนแปลง")','warning');
}
function cancelEditDept(){document.getElementById('editDeptPanel').style.display='none';}
function removeDeptAt(i){
  if(_wDepts[i].n==='อื่นๆ'){toast('ไม่สามารถลบ "อื่นๆ" ได้','warning');return;}
  _wDepts.splice(i,1);if(_selDeptIdx>=_wDepts.length)_selDeptIdx=_wDepts.length-1;
  renderDeptList();
}
function deleteSelectedDept(){if(_selDeptIdx<0){toast('กรุณาเลือกแผนกก่อน','warning');return;}removeDeptAt(_selDeptIdx);}
function moveDeptItem(dir){
  if(_selDeptIdx<0)return;var i=_selDeptIdx,j=i+dir;
  if(j<0||j>=_wDepts.length)return;
  var tmp=_wDepts[i];_wDepts[i]=_wDepts[j];_wDepts[j]=tmp;_selDeptIdx=j;renderDeptList();
}
function addDept(){
  var code=document.getElementById('newDeptCode').value.trim();
  var name=document.getElementById('newDeptName').value.trim();
  if(!name){toast('กรุณาระบุชื่อแผนก','error');return;}
  if(!code){var max=0;_wDepts.forEach(function(d){var n=parseInt(d.c,10);if(!isNaN(n)&&n>max&&n<9999)max=n;});code=String(max+1).padStart(4,'0');}
  if(_wDepts.some(function(d){return d.n===name;})){toast('ชื่อแผนกซ้ำ','warning');return;}
  // insert before อื่นๆ
  var lastIdx=_wDepts.findIndex(function(d){return d.n==='อื่นๆ';});
  if(lastIdx>=0)_wDepts.splice(lastIdx,0,{c:code,n:name});else _wDepts.push({c:code,n:name});
  document.getElementById('newDeptCode').value='';document.getElementById('newDeptName').value='';
  renderDeptList();toast('เพิ่มแผนก: '+name,'success');
}
function bulkAddDepts(){
  var lines=(document.getElementById('bulkDeptInput').value||'').split('\n').map(function(l){return l.trim();}).filter(Boolean);
  var added=0;var max=0;_wDepts.forEach(function(d){var n=parseInt(d.c,10);if(!isNaN(n)&&n>max&&n<9999)max=n;});
  var lastIdx=_wDepts.findIndex(function(d){return d.n==='อื่นๆ';});
  lines.forEach(function(name){
    if(_wDepts.some(function(d){return d.n===name;}))return;
    max++;var code=String(max).padStart(4,'0');
    if(lastIdx>=0){_wDepts.splice(lastIdx,0,{c:code,n:name});lastIdx++;}else _wDepts.push({c:code,n:name});
    added++;
  });
  document.getElementById('bulkDeptInput').value='';renderDeptList();
  toast('เพิ่ม '+added+' แผนก','success');
}
function applyDeptChanges(){
  if(!confirm('บันทึกรายชื่อแผนกใหม่ '+_wDepts.length+' รายการ?\nจะมีผลทันทีกับ Dropdown ทั้งหมด'))return;
  DEPTS=_wDepts.slice();_lsSet('depts',DEPTS);
  rebuildAllSelects();toast('บันทึกแผนกสำเร็จ! '+DEPTS.length+' รายการ','success');
}

// ─────────────────── STAFF ───────────────────
function renderStaffList(){
  if(!_wStaff)return;
  var q=(document.getElementById('staffFilterInput').value||'').toLowerCase();
  var area=document.getElementById('staffListArea');
  var badge=document.getElementById('staffCountBadge');if(badge)badge.textContent=_wStaff.length;
  var html=_wStaff.map(function(s,i){
    if(q&&!s.toLowerCase().includes(q))return'';
    var sel=_selStaffIdx===i;var isOther=s==='อื่นๆ';
    return'<div onclick="selectStaffItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(124,58,237,.12)':'transparent')+'" onmouseover="if(!'+sel+')this.style.background=\'rgba(255,255,255,.04)\'" onmouseout="if(!'+sel+')this.style.background=\'transparent\'">'
      +'<span style="flex:1;font-size:13px'+(isOther?';color:var(--mu)':'')+'">'+(isOther?'🔧 ':'👤 ')+s+'</span>'
      +(isOther?'<span style="font-size:10px;color:var(--mu)">ล็อค</span>':'<div style="display:flex;gap:4px"><button class="abt" style="font-size:10px;border-color:#f59e0b;color:#f59e0b" onclick="event.stopPropagation();startEditStaff('+i+')">✏️</button><button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeStaffAt('+i+')">✕</button></div>')
      +'</div>';
  }).join('');
  area.innerHTML=html||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectStaffItem(i){_selStaffIdx=i;renderStaffList();}
function startEditStaff(i){
  _selStaffIdx=i;
  document.getElementById('editStaffName').value=_wStaff[i];
  document.getElementById('editStaffPanel').style.display='block';
  renderStaffList();
}
function saveEditStaff(){
  if(_selStaffIdx<0)return;
  var name=document.getElementById('editStaffName').value.trim();
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  _wStaff[_selStaffIdx]=name;cancelEditStaff();renderStaffList();
  toast('แก้ไขชื่อแล้ว (ยังไม่บันทึก)','warning');
}
function cancelEditStaff(){document.getElementById('editStaffPanel').style.display='none';}
function removeStaffAt(i){
  if(_wStaff[i]==='อื่นๆ'){toast('ไม่สามารถลบ "อื่นๆ" ได้','warning');return;}
  _wStaff.splice(i,1);if(_selStaffIdx>=_wStaff.length)_selStaffIdx=-1;renderStaffList();
}
function deleteSelectedStaff(){if(_selStaffIdx<0){toast('กรุณาเลือกรายชื่อก่อน','warning');return;}removeStaffAt(_selStaffIdx);}
function moveStaffItem(dir){
  if(_selStaffIdx<0)return;var i=_selStaffIdx,j=i+dir;
  if(j<0||j>=_wStaff.length)return;
  var tmp=_wStaff[i];_wStaff[i]=_wStaff[j];_wStaff[j]=tmp;_selStaffIdx=j;renderStaffList();
}
function addStaff(){
  var prefix=document.getElementById('newStaffPrefix').value;
  var name=document.getElementById('newStaffName').value.trim();
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  var full=(prefix?prefix+' ':'')+name;
  if(_wStaff.includes(full)){toast('ชื่อซ้ำในรายการ','warning');return;}
  var lastIdx=_wStaff.indexOf('อื่นๆ');
  if(lastIdx>=0)_wStaff.splice(lastIdx,0,full);else _wStaff.push(full);
  document.getElementById('newStaffName').value='';document.getElementById('newStaffPrefix').value='';
  renderStaffList();toast('เพิ่ม: '+full,'success');
}
function bulkAddStaff(){
  var lines=(document.getElementById('bulkStaffInput').value||'').split('\n').map(function(l){return l.trim();}).filter(Boolean);
  var added=0;var lastIdx=_wStaff.indexOf('อื่นๆ');
  lines.forEach(function(name){
    if(_wStaff.includes(name))return;
    if(lastIdx>=0){_wStaff.splice(lastIdx,0,name);lastIdx++;}else _wStaff.push(name);
    added++;
  });
  document.getElementById('bulkStaffInput').value='';renderStaffList();
  toast('เพิ่ม '+added+' รายชื่อ','success');
}
function applyStaffChanges(){
  if(!confirm('บันทึกรายชื่อบุคลากรใหม่ '+_wStaff.length+' รายการ?\nจะมีผลทันทีกับ Dropdown ผู้ให้ยืม/ผู้รับคืน/ผู้โอน'))return;
  STAFF=_wStaff.slice();_lsSet('staff',STAFF);
  rebuildStaffSelects();toast('บันทึกรายชื่อบุคลากรสำเร็จ! '+STAFF.length+' รายการ','success');
}

// ─────────────────── EQUIPMENT ───────────────────
function renderEqList(){
  if(!_wEql)return;
  var q=(document.getElementById('eqFilterInput').value||'').toLowerCase();
  var area=document.getElementById('eqListArea');
  var badge=document.getElementById('eqCountBadge');if(badge)badge.textContent=_wEql.length;
  var html=_wEql.map(function(e,i){
    if(q&&!e.toLowerCase().includes(q))return'';
    var sel=_selEqlIdx2===i;
    return'<div onclick="selectEqItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(245,158,11,.12)':'transparent')+'">'
      +'<span style="flex:1;font-size:13px">🔧 '+e+'</span>'
      +'<div style="display:flex;gap:4px"><button class="abt" style="font-size:10px;border-color:#f59e0b;color:#f59e0b" onclick="event.stopPropagation();startEditEq('+i+')">✏️</button><button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeEqAt('+i+')">✕</button></div>'
      +'</div>';
  }).join('');
  area.innerHTML=html||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectEqItem(i){_selEqlIdx2=i;renderEqList();}
function startEditEq(i){
  _selEqlIdx2=i;
  document.getElementById('editEqName').value=_wEql[i];
  document.getElementById('editEqPanel').style.display='block';
  renderEqList();
}
function saveEditEq(){
  if(_selEqlIdx2<0)return;
  var name=document.getElementById('editEqName').value.trim();
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  _wEql[_selEqlIdx2]=name;cancelEditEq();renderEqList();
  toast('แก้ไขแล้ว (ยังไม่บันทึก)','warning');
}
function cancelEditEq(){document.getElementById('editEqPanel').style.display='none';}
function removeEqAt(i){
  if(_wEql[i]==='อื่นๆ'){toast('ไม่สามารถลบ "อื่นๆ" ได้','warning');return;}
  _wEql.splice(i,1);if(_selEqlIdx2>=_wEql.length)_selEqlIdx2=-1;renderEqList();
}
function deleteSelectedEq(){if(_selEqlIdx2<0){toast('กรุณาเลือกรายการก่อน','warning');return;}removeEqAt(_selEqlIdx2);}
function moveEqItem(dir){
  if(_selEqlIdx2<0)return;var i=_selEqlIdx2,j=i+dir;
  if(j<0||j>=_wEql.length)return;
  var tmp=_wEql[i];_wEql[i]=_wEql[j];_wEql[j]=tmp;_selEqlIdx2=j;renderEqList();
}
function addEqItem(){
  var name=document.getElementById('newEqName').value.trim();
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  if(_wEql.includes(name)){toast('ชื่อซ้ำ','warning');return;}
  var lastIdx=_wEql.indexOf('อื่นๆ');
  if(lastIdx>=0)_wEql.splice(lastIdx,0,name);else _wEql.push(name);
  document.getElementById('newEqName').value='';renderEqList();toast('เพิ่ม: '+name,'success');
}
function applyEqChanges(){
  if(!confirm('บันทึกรายการเครื่องมือ '+_wEql.length+' รายการ?\nจะมีผลทันทีกับการ์ดเลือกเครื่องมือและ Dropdown ทั้งหมด'))return;
  EQL=_wEql.slice();_lsSet('eql',EQL);
  rebuildEqCards();rebuildEqSelects();
  toast('บันทึกรายการเครื่องมือสำเร็จ! '+EQL.length+' รายการ','success');
}

// ─────────────────── ACCESSORIES ───────────────────
function _accTypeLabel(a){if(a.no)return'No.';if(a.det)return'ข้อความ';return'Tick';}
function renderAccList(){
  if(!_wAcc)return;
  var area=document.getElementById('accListArea');
  var badge=document.getElementById('accCountBadge');if(badge)badge.textContent=_wAcc.length;
  var html=_wAcc.map(function(a,i){
    var sel=_selAccIdx===i;
    var typeCol={No:'var(--ac)','ข้อความ':'var(--ac2)','Tick':'var(--mu)'}[_accTypeLabel(a)]||'var(--mu)';
    return'<div onclick="selectAccItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(16,185,129,.1)':'transparent')+'">'
      +'<span style="flex:1;font-size:13px">📦 '+a.n+'</span>'
      +'<span style="font-size:10px;font-family:var(--mo);color:'+typeCol+';background:rgba(0,0,0,.2);padding:1px 5px;border-radius:3px">'+_accTypeLabel(a)+'</span>'
      +'<div style="display:flex;gap:4px"><button class="abt" style="font-size:10px;border-color:#f59e0b;color:#f59e0b" onclick="event.stopPropagation();startEditAcc('+i+')">✏️</button><button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeAccAt('+i+')">✕</button></div>'
      +'</div>';
  }).join('');
  area.innerHTML=html||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectAccItem(i){_selAccIdx=i;renderAccList();}
function startEditAcc(i){
  _selAccIdx=i;
  document.getElementById('editAccName').value=_wAcc[i].n;
  var t=_wAcc[i].no?'no':_wAcc[i].det?'det':'none';
  document.getElementById('editAccType').value=t;
  document.getElementById('editAccPanel').style.display='block';
  renderAccList();
}
function saveEditAcc(){
  if(_selAccIdx<0)return;
  var name=document.getElementById('editAccName').value.trim();var t=document.getElementById('editAccType').value;
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  _wAcc[_selAccIdx].n=name;_wAcc[_selAccIdx].no=t==='no';_wAcc[_selAccIdx].det=t==='det';
  cancelEditAcc();renderAccList();toast('แก้ไขแล้ว (ยังไม่บันทึก)','warning');
}
function cancelEditAcc(){document.getElementById('editAccPanel').style.display='none';}
function removeAccAt(i){_wAcc.splice(i,1);if(_selAccIdx>=_wAcc.length)_selAccIdx=-1;renderAccList();}
function deleteSelectedAcc(){if(_selAccIdx<0){toast('กรุณาเลือกอุปกรณ์ก่อน','warning');return;}removeAccAt(_selAccIdx);}
function moveAccItem(dir){
  if(_selAccIdx<0)return;var i=_selAccIdx,j=i+dir;
  if(j<0||j>=_wAcc.length)return;
  var tmp=_wAcc[i];_wAcc[i]=_wAcc[j];_wAcc[j]=tmp;_selAccIdx=j;renderAccList();
}
function addAccItem(){
  var name=document.getElementById('newAccName').value.trim();var t=document.getElementById('newAccType').value;
  if(!name){toast('กรุณาระบุชื่อ','error');return;}
  var maxId=0;_wAcc.forEach(function(a){var n=parseInt((a.id||'a0').replace('a',''),10);if(!isNaN(n)&&n>maxId)maxId=n;});
  var newId='a'+(maxId+1);
  _wAcc.push({id:newId,n:name,no:t==='no',det:t==='det'});
  document.getElementById('newAccName').value='';renderAccList();toast('เพิ่มอุปกรณ์: '+name,'success');
}
function applyAccChanges(){
  if(!confirm('บันทึกรายการอุปกรณ์ประกอบ '+_wAcc.length+' รายการ?\nจะมีผลทันทีกับตาราง "อุปกรณ์ประกอบ" ตอนบันทึกการยืม'))return;
  ACCS=_wAcc.map(function(a){return{id:a.id,n:a.n,no:a.no||undefined,det:a.det||undefined};});
  _lsSet('accs',ACCS);
  buildAccTable();
  toast('บันทึกอุปกรณ์ประกอบสำเร็จ! '+ACCS.length+' รายการ','success');
}

// ── Export JSON ──
function exportListJSON(type){
  var data,fname;
  if(type==='dept'){data=_wDepts||DEPTS;fname='depts';}
  else if(type==='staff'){data=_wStaff||STAFF;fname='staff';}
  else if(type==='eq'){data=_wEql||EQL;fname='equipment';}
  else{data=_wAcc||ACCS;fname='accessories';}
  var blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
  var a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=fname+'_'+new Date().toISOString().split('T')[0]+'.json';a.click();
  toast('Export '+fname+' JSON สำเร็จ');
}

// ── Rebuild all dropdowns/UI after applying changes ──
// ─────────────── EQNUM MANAGER ───────────────────────
var _wEqnum=null, _selEqnumIdx=-1;

function renderEqnumList(){
  if(!_wEqnum)return;
  var area=document.getElementById('eqnumListArea');
  var badge=document.getElementById('eqnumCountBadge');
  if(badge)badge.textContent=_wEqnum.length;
  area.innerHTML=_wEqnum.map(function(n,i){
    var sel=_selEqnumIdx===i;
    return'<div onclick="selectEqnumItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(0,200,255,.1)':'transparent')+'">'
      +'<span style="flex:1;font-size:13px;font-family:var(--mo)">'+n+'</span>'
      +'<button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeEqnumAt('+i+')">✕</button>'
      +'</div>';
  }).join('')||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectEqnumItem(i){_selEqnumIdx=i;renderEqnumList();}
function removeEqnumAt(i){_wEqnum.splice(i,1);if(_selEqnumIdx>=_wEqnum.length)_selEqnumIdx=-1;renderEqnumList();}
function deleteSelectedEqnum(){if(_selEqnumIdx<0){toast('กรุณาเลือกรายการก่อน','warning');return;}removeEqnumAt(_selEqnumIdx);}
function moveEqnumItem(dir){if(_selEqnumIdx<0)return;var i=_selEqnumIdx,j=i+dir;if(j<0||j>=_wEqnum.length)return;var t=_wEqnum[i];_wEqnum[i]=_wEqnum[j];_wEqnum[j]=t;_selEqnumIdx=j;renderEqnumList();}
function addEqnumItem(){
  var n=(document.getElementById('newEqnumName').value||'').trim();
  if(!n){toast('กรุณาระบุหมายเลข','error');return;}
  if(_wEqnum.includes(n)){toast('มีหมายเลขนี้อยู่แล้ว','warning');return;}
  _wEqnum.push(n);document.getElementById('newEqnumName').value='';
  renderEqnumList();toast('เพิ่ม: '+n,'success');
}
function bulkAddEqnums(){
  var raw=document.getElementById('bulkEqnumInput').value||'';
  var lines=raw.split(/[\n\r]+/).map(function(l){return l.trim();}).filter(Boolean);
  var added=0;lines.forEach(function(n){if(!_wEqnum.includes(n)){_wEqnum.push(n);added++;}});
  document.getElementById('bulkEqnumInput').value='';renderEqnumList();toast('เพิ่ม '+added+' รายการ','success');
}
function applyEqnumChanges(){
  if(!confirm('บันทึกหมายเลขเครื่องมือ '+_wEqnum.length+' รายการ?'))return;
  EQNUMS=_wEqnum.slice();_lsSet('eqnums',EQNUMS);
  rebuildEqNoDatalist();toast('บันทึกสำเร็จ! '+EQNUMS.length+' รายการ','success');
}

// ─────────────── RETREM MANAGER ───────────────────────
var _wRetrem=null, _selRetremIdx=-1;

function renderRetremList(){
  if(!_wRetrem)return;
  var area=document.getElementById('retremListArea');
  var badge=document.getElementById('retremCountBadge');
  if(badge)badge.textContent=_wRetrem.length;
  area.innerHTML=_wRetrem.map(function(r,i){
    var sel=_selRetremIdx===i;
    var isOther=r.label==='อื่นๆ';
    return'<div onclick="selectRetremItem('+i+')" style="display:flex;align-items:center;gap:8px;padding:8px 12px;border-bottom:1px solid var(--bd);cursor:pointer;background:'+(sel?'rgba(16,185,129,.1)':'transparent')+'">'
      +'<span style="flex:1;font-size:13px">'+r.label+'</span>'
      +(r.needDetail?'<span style="font-size:10px;background:rgba(245,158,11,.15);color:var(--wn);border-radius:4px;padding:1px 5px">+รายละเอียด</span>':'')
      +(isOther?'<span style="font-size:10px;color:var(--mu)">ล็อค</span>':'<button class="abt" style="font-size:10px;border-color:var(--er);color:var(--er)" onclick="event.stopPropagation();removeRetremAt('+i+')">✕</button>')
      +'</div>';
  }).join('')||'<div style="text-align:center;padding:20px;color:var(--mu);font-size:12px">ไม่มีรายการ</div>';
}
function selectRetremItem(i){_selRetremIdx=i;renderRetremList();}
function removeRetremAt(i){
  if(_wRetrem[i]&&_wRetrem[i].label==='อื่นๆ'){toast('ไม่สามารถลบ "อื่นๆ" ได้','warning');return;}
  _wRetrem.splice(i,1);if(_selRetremIdx>=_wRetrem.length)_selRetremIdx=-1;renderRetremList();
}
function deleteSelectedRetrem(){if(_selRetremIdx<0){toast('กรุณาเลือกรายการก่อน','warning');return;}removeRetremAt(_selRetremIdx);}
function moveRetremItem(dir){if(_selRetremIdx<0)return;var i=_selRetremIdx,j=i+dir;if(j<0||j>=_wRetrem.length)return;var t=_wRetrem[i];_wRetrem[i]=_wRetrem[j];_wRetrem[j]=t;_selRetremIdx=j;renderRetremList();}
function addRetremItem(){
  var label=(document.getElementById('newRetremLabel').value||'').trim();
  var needDetail=document.getElementById('newRetremDetail').value==='1';
  if(!label){toast('กรุณาระบุเหตุผล','error');return;}
  if(_wRetrem.some(function(r){return r.label===label;})){toast('มีเหตุผลนี้อยู่แล้ว','warning');return;}
  var lastIdx=_wRetrem.findIndex(function(r){return r.label==='อื่นๆ';});
  var obj={label:label,needDetail:needDetail};
  if(lastIdx>=0)_wRetrem.splice(lastIdx,0,obj);else _wRetrem.push(obj);
  document.getElementById('newRetremLabel').value='';
  renderRetremList();toast('เพิ่ม: '+label,'success');
}
function applyRetremChanges(){
  if(!confirm('บันทึกเหตุผลการรับคืน '+_wRetrem.length+' รายการ?'))return;
  RETREMARKS=_wRetrem.map(function(r){return r.label;});
  _lsSet('retremarks',RETREMARKS);
  rebuildRetRemarkSelect();toast('บันทึกสำเร็จ! '+RETREMARKS.length+' รายการ','success');
}

// ─────────────── HFNC SET ADMIN ──────────────────────

// default HFNC set config stored in localStorage
var _HFNC_SET_DEFAULT=[
  {id:'airvo2',label:'Airvo2',color:'#3266ad',ranges:[{from:1,to:25},{from:36,to:38},{from:59,to:70}],named:[]},
  {id:'heyer', label:'Heyer', color:'#7c3aed',ranges:[{from:26,to:35},{from:39,to:58}],named:[]},
  {id:'o2flo', label:'O2FLO', color:'#10b981',ranges:[],named:['O2FLO1','O2FLO2','O2FLO3']},
  {id:'mek',   label:'MEK',   color:'#f59e0b',ranges:[],named:['MEK1','MEK2','MEK3','MEK4','MEK5']}
];
function _hfncSetGet(){try{var v=localStorage.getItem('meq_hfncset');return v?JSON.parse(v):JSON.parse(JSON.stringify(_HFNC_SET_DEFAULT));}catch(e){return JSON.parse(JSON.stringify(_HFNC_SET_DEFAULT));}}
function _hfncSetSave(d){try{localStorage.setItem('meq_hfncset',JSON.stringify(d));}catch(e){}}

function renderHfncSetAdmin(){
  var area=document.getElementById('hfncSetAdminArea');if(!area)return;
  var sets=_hfncSetGet();
  var html=sets.map(function(s,si){
    var rangesStr=s.ranges.map(function(r){return r.from+'-'+r.to;}).join(', ');
    var namedStr=s.named.join(', ');
    return'<div style="background:var(--s2);border:1px solid var(--bd);border-radius:8px;padding:14px 16px;margin-bottom:10px;border-left:4px solid '+s.color+'">'
      +'<div style="display:flex;align-items:center;gap:10px;margin-bottom:10px">'
        +'<span style="width:14px;height:14px;border-radius:3px;background:'+s.color+';flex-shrink:0"></span>'
        +'<input type="text" value="'+s.label+'" id="hfncSetLabel_'+si+'" style="font-size:14px;font-weight:700;color:'+s.color+';background:transparent;border:none;border-bottom:1px dashed var(--bd);padding:2px 4px;width:120px;font-family:var(--fn)" onchange="hfncSetUpdateLabel('+si+',this.value)">'
        +'<input type="color" value="'+s.color+'" id="hfncSetColor_'+si+'" onchange="hfncSetUpdateColor('+si+',this.value)" style="width:30px;height:26px;padding:2px;border:1px solid var(--bd);border-radius:4px;background:var(--bg);cursor:pointer">'
      +'</div>'
      +'<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px">'
        +'<div>'
          +'<label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">ช่วงหมายเลข (เช่น 1-25, 36-38)</label>'
          +'<input type="text" id="hfncSetRanges_'+si+'" value="'+rangesStr+'" placeholder="เช่น 1-25, 36-38, 59-70" style="font-size:12px;font-family:var(--mo)">'
          +'<div style="font-size:10px;color:var(--mu);margin-top:3px">คั่นด้วย comma ถ้ามีหลายช่วง</div>'
        +'</div>'
        +'<div>'
          +'<label style="font-size:10px;font-weight:700;color:var(--mu);text-transform:uppercase;letter-spacing:.4px;display:block;margin-bottom:4px">หมายเลขแบบข้อความ (เช่น O2FLO1, MEK1)</label>'
          +'<input type="text" id="hfncSetNamed_'+si+'" value="'+namedStr+'" placeholder="เช่น O2FLO1, MEK1" style="font-size:12px;font-family:var(--mo)">'
          +'<div style="font-size:10px;color:var(--mu);margin-top:3px">คั่นด้วย comma, จับคู่ตรงๆ (case-insensitive)</div>'
        +'</div>'
      +'</div>'
      // preview chips
      +'<div style="margin-top:10px;padding-top:8px;border-top:1px solid var(--bd)">'
        +'<div style="font-size:10px;color:var(--mu);margin-bottom:4px">ตัวอย่างหมายเลขที่จัดกลุ่ม:</div>'
        +'<div id="hfncSetPreview_'+si+'" style="font-size:11px;color:var(--mu);font-family:var(--mo)">'+_hfncSetPreviewText(s)+'</div>'
      +'</div>'
    +'</div>';
  }).join('');
  // add new set button
  html+='<button class="btn bg sm" onclick="hfncSetAddNew()" style="width:100%;margin-top:4px">➕ เพิ่ม Set ใหม่</button>';
  area.innerHTML=html;
}

function _hfncSetPreviewText(s){
  var parts=[];
  s.ranges.forEach(function(r){
    var nums=[];for(var i=r.from;i<=r.to;i++)nums.push(i);
    parts=parts.concat(nums.slice(0,5));
    if(nums.length>5)parts.push('...'+r.to);
  });
  s.named.forEach(function(n){parts.push(n);});
  if(!parts.length)return'(ยังไม่มีหมายเลข)';
  return parts.slice(0,15).join(', ')+(parts.length>15?' ...':'');
}

function _parseRanges(str){
  var out=[];
  (str||'').split(',').forEach(function(part){
    part=part.trim();if(!part)return;
    var m=part.match(/^(\d+)\s*-\s*(\d+)$/);
    if(m){out.push({from:parseInt(m[1],10),to:parseInt(m[2],10)});}
    else if(/^\d+$/.test(part)){var n=parseInt(part,10);out.push({from:n,to:n});}
  });
  return out;
}
function _parseNamed(str){
  return (str||'').split(',').map(function(s){return s.trim();}).filter(Boolean);
}

function hfncSetUpdateLabel(si,val){
  var sets=_hfncSetGet();if(!sets[si])return;
  sets[si].label=val.trim();_hfncSetSave(sets);
}
function hfncSetUpdateColor(si,val){
  var sets=_hfncSetGet();if(!sets[si])return;
  sets[si].color=val;_hfncSetSave(sets);
  var lbl=document.getElementById('hfncSetLabel_'+si);
  if(lbl)lbl.style.color=val;
}
function hfncSetAddNew(){
  var sets=_hfncSetGet();
  sets.push({id:'set'+Date.now(),label:'Set ใหม่',color:'#8b949e',ranges:[],named:[]});
  _hfncSetSave(sets);renderHfncSetAdmin();
}

function applyHfncSetChanges(){
  var sets=_hfncSetGet();
  var valid=true;
  sets.forEach(function(s,si){
    var labelEl=document.getElementById('hfncSetLabel_'+si);
    var rangesEl=document.getElementById('hfncSetRanges_'+si);
    var namedEl=document.getElementById('hfncSetNamed_'+si);
    if(labelEl)s.label=labelEl.value.trim();
    if(rangesEl)s.ranges=_parseRanges(rangesEl.value);
    if(namedEl)s.named=_parseNamed(namedEl.value);
    if(!s.label){toast('กรุณาใส่ชื่อ Set ทุก set','error');valid=false;}
  });
  if(!valid)return;
  _hfncSetSave(sets);
  // อัปเดต window._buildHfncSetHtml ให้ใช้ config ใหม่
  _patchHfncSetMatcher(sets);
  toast('บันทึก HFNC Set สำเร็จ! '+sets.length+' sets','success');
  renderHfncSetAdmin();
  // re-render section สถิติถ้าเปิดอยู่
  if(window._renderHfncSection)window._renderHfncSection();
}

function _patchHfncSetMatcher(sets){
  // patch SET_DEFS ใน _buildHfncSetHtml โดยสร้าง wrapper function ใหม่
  var _origBuild=window._buildHfncSetHtml;
  window._buildHfncSetHtml=function(filterDateS,filterDateE){
    // re-build SET_DEFS จาก localStorage
    var _sets=_hfncSetGet();
    var NEW_SET_DEFS=_sets.map(function(s){
      return{
        id:s.id,label:s.label,color:s.color,bg:s.color.replace('#','rgba(').replace(/([0-9a-f]{2})([0-9a-f]{2})([0-9a-f]{2})/i,function(_,r,g,b){return parseInt(r,16)+','+parseInt(g,16)+','+parseInt(b,16)+',.15)'}),
        match:function(no){
          var n=parseInt(no,10);
          for(var i=0;i<s.ranges.length;i++){if(!isNaN(n)&&n>=s.ranges[i].from&&n<=s.ranges[i].to)return true;}
          for(var j=0;j<s.named.length;j++){if((no||'').trim().toLowerCase()===s.named[j].toLowerCase())return true;}
          return false;
        }
      };
    });
    // call original logic with new SET_DEFS injected via closure trick
    // ใช้ตรงๆ จาก logic เดิมแต่แทน SET_DEFS
    var SET_DEFS=NEW_SET_DEFS;
    var setStats={};
    SET_DEFS.forEach(function(s){setStats[s.id]={machines:{},totalLoans:0,activeNow:0};});
    var unknownMachines={};
    expanded().forEach(function(item){
      if(S(item.eq.name)!=='HFNC')return;
      if(!item.borrow)return;
      if(item.status==='PENDING'||item.status==='CANCELLED')return;
      var ld=item.loan.LoanDate||'';
      if(filterDateS&&ld&&ld<filterDateS)return;
      if(filterDateE&&ld&&ld>filterDateE)return;
      var eqNo=S(item.borrow.EquipmentNo).trim();
      var dept=S(item.loan.DeptName);
      var isActive=item.status==='ACTIVE'||item.status==='ACC_PENDING';
      var matched=false;
      SET_DEFS.forEach(function(s){
        if(s.match(eqNo)){
          matched=true;
          var st=setStats[s.id];
          st.totalLoans++;if(isActive)st.activeNow++;
          if(!st.machines[eqNo])st.machines[eqNo]={count:0,active:false,dept:''};
          st.machines[eqNo].count++;
          if(isActive){st.machines[eqNo].active=true;st.machines[eqNo].dept=dept;}
        }
      });
      if(!matched&&eqNo&&eqNo!=='—'&&eqNo!=='-'){if(!unknownMachines[eqNo])unknownMachines[eqNo]=0;unknownMachines[eqNo]++;}
    });
    var totalActive=0,totalLoans=0;
    SET_DEFS.forEach(function(s){totalActive+=setStats[s.id].activeNow;totalLoans+=setStats[s.id].totalLoans;});
    var cards=SET_DEFS.map(function(s){
      var st=setStats[s.id];var machCount=Object.keys(st.machines).length;
      var pct=machCount>0?Math.round(st.activeNow/machCount*100):0;
      return'<div style="background:var(--s2);border:1px solid var(--bd);border-radius:8px;padding:12px 14px;border-top:3px solid '+s.color+'">'
        +'<div style="font-size:13px;font-weight:700;color:'+s.color+';margin-bottom:2px">'+s.label+'</div>'
        +'<div style="font-size:26px;font-weight:700;font-family:var(--mo);color:var(--tx)">'+st.activeNow+'</div>'
        +'<div style="font-size:11px;color:var(--mu)">กำลังยืมอยู่</div>'
        +'<div style="margin-top:6px;font-size:11px;color:var(--mu)">ยืมทั้งหมด <strong style="color:var(--tx)">'+st.totalLoans+'</strong> ครั้ง | <strong style="color:var(--tx)">'+machCount+'</strong> เครื่อง</div>'
        +'<div style="margin-top:8px;height:4px;background:var(--bg);border-radius:2px;overflow:hidden"><div style="height:100%;width:'+pct+'%;background:'+s.color+';border-radius:2px;transition:width .5s"></div></div>'
        +'</div>';
    }).join('');
    var donutData=SET_DEFS.map(function(s){return{label:s.label,val:setStats[s.id].totalLoans,color:s.color};});
    var donutTotal=donutData.reduce(function(s,d){return s+d.val;},0)||1;
    var donutR=60,donutCx=75,donutCy=75,donutInner=38;
    var donutPaths='';var cumAngle=-Math.PI/2;
    donutData.forEach(function(d){
      if(d.val===0)return;
      var angle=d.val/donutTotal*Math.PI*2;
      var x1=donutCx+donutR*Math.cos(cumAngle);var y1=donutCy+donutR*Math.sin(cumAngle);
      var x2=donutCx+donutR*Math.cos(cumAngle+angle);var y2=donutCy+donutR*Math.sin(cumAngle+angle);
      var xi1=donutCx+donutInner*Math.cos(cumAngle);var yi1=donutCy+donutInner*Math.sin(cumAngle);
      var xi2=donutCx+donutInner*Math.cos(cumAngle+angle);var yi2=donutCy+donutInner*Math.sin(cumAngle+angle);
      var lg=angle>Math.PI?1:0;
      donutPaths+='<path d="M'+xi1+' '+yi1+' L'+x1+' '+y1+' A'+donutR+' '+donutR+' 0 '+lg+' 1 '+x2+' '+y2+' L'+xi2+' '+yi2+' A'+donutInner+' '+donutInner+' 0 '+lg+' 0 '+xi1+' '+yi1+' Z" fill="'+d.color+'" opacity=".9"><title>'+d.label+' '+d.val+' ครั้ง</title></path>';
      cumAngle+=angle;
    });
    var donutSvg='<svg width="150" height="150" viewBox="0 0 150 150" xmlns="http://www.w3.org/2000/svg">'+donutPaths+'<text x="75" y="72" text-anchor="middle" fill="var(--tx)" font-size="18" font-weight="700" font-family="IBM Plex Mono,monospace">'+totalActive+'</text><text x="75" y="87" text-anchor="middle" fill="#8b949e" font-size="10">กำลังยืม</text></svg>';
    var legend=SET_DEFS.map(function(s){var pct=donutTotal>0?Math.round(setStats[s.id].totalLoans/donutTotal*100):0;return'<div style="display:flex;align-items:center;gap:6px;font-size:12px"><span style="display:inline-block;width:10px;height:10px;border-radius:2px;background:'+s.color+';flex-shrink:0"></span><span style="color:var(--tx)">'+s.label+'</span><span style="color:var(--mu);margin-left:auto">'+setStats[s.id].totalLoans+' ครั้ง ('+pct+'%)</span></div>';}).join('');
    var machineGrids=SET_DEFS.map(function(s){
      var st=setStats[s.id];var allMachKeys=Object.keys(st.machines).sort(function(a,b){var na=parseInt(a,10),nb=parseInt(b,10);if(!isNaN(na)&&!isNaN(nb))return na-nb;return a.localeCompare(b);});
      var chips=allMachKeys.map(function(eqNo){var m=st.machines[eqNo];var isAct=m.active;return'<span title="'+(isAct?'กำลังยืม: '+m.dept:'ยืมแล้ว '+m.count+' ครั้ง')+'" style="display:inline-flex;align-items:center;gap:3px;font-size:10px;padding:2px 6px;border-radius:4px;font-family:var(--mo);background:'+(isAct?s.bg:'rgba(139,148,158,.1)')+';color:'+(isAct?s.color:'#8b949e')+';border:1px solid '+(isAct?'rgba(139,148,158,.4)':'rgba(139,148,158,.25)')+'" >'+eqNo+(isAct?'<span style="width:5px;height:5px;border-radius:50%;background:'+s.color+';flex-shrink:0"></span>':'')+'</span>';}).join('');
      if(!allMachKeys.length)chips='<span style="font-size:11px;color:var(--mu)">ยังไม่มีข้อมูล</span>';
      return'<div style="background:var(--bg);border:1px solid var(--bd);border-radius:7px;padding:10px 12px;border-left:3px solid '+s.color+'"><div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px"><span style="font-size:12px;font-weight:700;color:'+s.color+'">'+s.label+'</span><span style="font-size:11px;color:var(--mu)">ยืม '+st.totalLoans+' ครั้ง | active '+st.activeNow+'</span></div><div style="display:flex;flex-wrap:wrap;gap:4px">'+chips+'</div></div>';
    }).join('');
    var unknownKeys=Object.keys(unknownMachines);
    var unknownHtml=unknownKeys.length?'<div style="margin-top:10px;padding:8px 12px;background:rgba(239,68,68,.06);border:1px solid rgba(239,68,68,.2);border-radius:6px;font-size:11px;color:var(--mu)">⚠️ ไม่อยู่ใน set ใด: '+unknownKeys.map(function(k){return'<span style="font-family:var(--mo);background:rgba(239,68,68,.1);padding:1px 5px;border-radius:3px;color:var(--er)">'+k+'</span>';}).join(' ')+'</div>':'';
    return{cards:cards,donutSvg:donutSvg,legend:legend,machineGrids:machineGrids,unknownHtml:unknownHtml,totalLoans:totalLoans,totalActive:totalActive,filterLabel:''};
  };
}

// init patch on load
setTimeout(function(){_patchHfncSetMatcher(_hfncSetGet());},100);

// ─────────────── REBUILD HELPERS ──────────────────────
function rebuildEqNoDatalist(){
  var dl=document.getElementById('eqNoList');if(!dl)return;
  dl.innerHTML=EQNUMS.map(function(n){return'<option value="'+n+'">';}).join('');
}
function rebuildRetRemarkSelect(){
  var sel=document.getElementById('retRemark');if(!sel)return;
  var cur=sel.value;
  sel.innerHTML='<option value="">เลือกเหตุผล</option>';
  RETREMARKS.forEach(function(r){sel.innerHTML+='<option value="'+r+'">'+r+'</option>';});
  if(RETREMARKS.includes(cur))sel.value=cur;
}

function rebuildAllSelects(){
  rebuildEqNoDatalist();
  rebuildRetRemarkSelect();
  // dept dropdowns in filters
  ['allDept','recDept','dailyDept'].forEach(function(id){
    var el=document.getElementById(id);if(!el)return;
    var v=el.value;el.innerHTML='<option value="">ทั้งแผนก</option>';
    DEPTS.forEach(function(d){el.innerHTML+='<option value="'+d.n+'">'+d.n+'</option>';});
    el.value=v;
  });
  // rebuild SSL lists
  buildSSL('deptList',DEPTS,function(c,n){pickDept('deptList',c,n);});
  buildSSL('trDeptList',DEPTS,function(c,n){pickDept('trDeptList',c,n);});
}
function rebuildStaffSelects(){
  ['giverSel','receiverSel','transfererSel'].forEach(function(id){
    var el=document.getElementById(id);if(!el)return;
    var v=el.value;el.innerHTML='<option value="">— เลือก —</option>';
    STAFF.forEach(function(s){el.innerHTML+='<option value="'+s+'">'+s+'</option>';});
    el.value=STAFF.includes(v)?v:'';
  });
}
function rebuildEqCards(){
  var grid=document.getElementById('eqCardGrid');if(!grid)return;
  grid.innerHTML='';_selEqIdx=-1;buildEqCards();
}
function rebuildEqSelects(){
  ['allEq','recEq','dailyFilter','addLoanEq','admBorEq'].forEach(function(id){
    var el=document.getElementById(id);if(!el)return;
    var v=el.value;var firstOpt=el.options[0]?el.options[0].outerHTML:'';
    el.innerHTML=firstOpt;
    EQL.forEach(function(e){el.innerHTML+='<option value="'+e+'">'+e+'</option>';});
    el.value=EQL.includes(v)?v:'';
  });
}

function exportAllJSON(){
  if(!loans||!borrows){toast('ไม่มีข้อมูล','warning');return;}
  var data={loans:loans,borrows:borrows,exportedAt:new Date().toISOString()};
  var blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
  var a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download='medical_equipment_export_'+new Date().toISOString().split('T')[0]+'.json';a.click();
  toast('Export JSON สำเร็จ');
}
function dlAdmBorrows(){
  if(!borrows){toast('ไม่มีข้อมูล','warning');return;}
  var rows=[['RecordID','LoanID','GiverName','EquipmentNo','ReturnDate','ReceiverName','ReturnRemark','Status','MachineReturned','Accessories']];
  borrows.forEach(function(b){rows.push([S(b.RecordID),S(b.LoanID),S(b.GiverName),S(b.EquipmentNo),S(b.ReturnDate),S(b.ReceiverName),S(b.ReturnRemark),S(b.Status),b.MachineReturned?'true':'false',(b.Accessories||[]).map(function(a){return a.name+(a.no?' No.'+a.no:'');}).join('; ')]);});
  var csv=rows.map(function(r){return r.map(function(c){return'"'+String(c).replace(/"/g,'""')+'"';}).join(',');}).join('\n');
  dlCSV(csv,'borrow_records_'+new Date().toISOString().split('T')[0]+'.csv');
}

// ════════════════════════════════════════════════════════════
// SHARED DOWNLOAD / UI HELPERS
// ════════════════════════════════════════════════════════════
function getPageData(page){
  var rows=[],now=new Date();var exp=expanded();
  if(page==='rec'||page==='all'){
    var sf=page==='rec'?document.getElementById('recStat').value:document.getElementById('allStat').value;var feq=page==='rec'?document.getElementById('recEq').value:document.getElementById('allEq').value;var fdept=page==='rec'?document.getElementById('recDept').value:document.getElementById('allDept').value;var search=page==='rec'?document.getElementById('recSearch').value.toLowerCase():document.getElementById('allSearch').value.toLowerCase();var ds=page==='rec'?document.getElementById('recDateS').value:document.getElementById('allDateS').value;var de=page==='rec'?document.getElementById('recDateE').value:document.getElementById('allDateE').value;
    var items=sortItems(exp.filter(function(item){if(sf&&item.status!==sf)return false;if(feq&&S(item.eq.name)!==feq)return false;if(fdept&&S(item.loan.DeptName)!==fdept)return false;var ld=item.loan.LoanDate;if(ds&&ld&&ld<ds)return false;if(de&&ld&&ld>de)return false;var txt=S(item.loan.LoanID)+' '+S(item.loan.BorrowerName)+' '+S(item.loan.DeptName)+' '+S(item.loan.PatientName)+' '+S(item.eq.name)+' '+(item.borrow?S(item.borrow.EquipmentNo):'');return!search||txt.toLowerCase().includes(search);}));
    rows.push(['LoanID','วันที่ยืม','ผู้ยืม','แผนก','ผู้ป่วย','เครื่องมือ','หมายเลขเครื่อง','อุปกรณ์ประกอบ','วันที่ใช้','สถานะ','ผู้ให้ยืม','ผู้รับคืน','วันที่คืน','เหตุผลคืน','ประวัติโอน']);
    items.forEach(function(item){var ld=item.loan.LoanDate,startD=ld?new Date(ld):null,endD=item.borrow&&item.borrow.ReturnDate?new Date(item.borrow.ReturnDate):now;var days=startD&&(item.status==='ACTIVE'||item.status==='RETURNED'||item.status==='ACC_PENDING')?Math.max(0,Math.round((endD-startD)/86400000)):'-';var accs=item.borrow&&Array.isArray(item.borrow.Accessories)?item.borrow.Accessories:[];var th=item.borrow&&Array.isArray(item.borrow.TransferHistory)?item.borrow.TransferHistory:[];rows.push([S(item.loan.LoanID),ld||'',S(item.loan.BorrowerName),S(item.loan.DeptName),S(item.loan.PatientName),S(item.eq.name),item.borrow?S(item.borrow.EquipmentNo||''):'-',accs.length||0,days,({PENDING:'รอดำเนินการจ่าย',ACTIVE:'ยืม',RETURNED:'คืน',ACC_PENDING:'คืน (อุปกรณ์ค้าง)',CANCELLED:'ยกเลิก'}[item.status]||item.status),item.borrow?S(item.borrow.GiverName||''):'-',item.borrow?S(item.borrow.ReceiverName||''):'-',item.borrow?S(item.borrow.ReturnDate||''):'-',item.borrow?S(item.borrow.ReturnRemark||''):'-',th.map(function(t){return t.date+':'+S(t.toDept);}).join('→')]);});
  }else if(page==='daily'){var filter=document.getElementById('dailyFilter').value;var activeItems=exp.filter(function(i){return i.status==='ACTIVE'&&(!filter||S(i.eq.name)===filter);});rows.push(['หมายเลขเครื่อง','ชนิดเครื่องมือ','วันที่ยืม','แผนก','ผู้ป่วย','วันที่ใช้','ผู้ให้ยืม','ประวัติโอน','อุปกรณ์ประกอบ']);activeItems.forEach(function(item){var ld=item.loan.LoanDate||'-',days=ld!=='-'?Math.max(0,Math.round((now-new Date(ld))/86400000)):'-';var th=item.borrow&&item.borrow.TransferHistory?item.borrow.TransferHistory:[];rows.push([item.borrow?S(item.borrow.EquipmentNo||'—'):'—',S(item.eq.name),ld,S(item.loan.DeptName),S(item.loan.PatientName),days,item.borrow?S(item.borrow.GiverName||'-'):'-',th.map(function(t){return S(t.toDept)+'('+t.date+')';}).join('→'),(item.borrow&&item.borrow.Accessories?item.borrow.Accessories.length:0)]);});}
  else if(page==='sum'){
    rows.push(['ชนิดเครื่องมือ','กำลังยืม (ACTIVE)','คืนแล้ว','รอดำเนินการ','ค้างอุปกรณ์','รวมทั้งหมด']);
    var rangeS2=(document.getElementById('sumS')||{}).value||'';var rangeE2=(document.getElementById('sumE')||{}).value||'';
    var filteredSum=exp.filter(function(i){var ld=i.loan.LoanDate;if(rangeS2&&ld&&ld<rangeS2)return false;if(rangeE2&&ld&&ld>rangeE2)return false;return true;});
    var eqTotals={};filteredSum.forEach(function(i){var n=S(i.eq.name);if(!eqTotals[n])eqTotals[n]={active:0,ret:0,pend:0,acc:0,total:0};eqTotals[n].total++;if(i.status==='ACTIVE')eqTotals[n].active++;else if(i.status==='RETURNED')eqTotals[n].ret++;else if(i.status==='PENDING')eqTotals[n].pend++;else if(i.status==='ACC_PENDING')eqTotals[n].acc++;});
    Object.entries(eqTotals).sort(function(a,b){return b[1].total-a[1].total;}).forEach(function(e){rows.push([e[0],e[1].active,e[1].ret,e[1].pend,e[1].acc,e[1].total]);});
  }
  else if(page==='dash'){
    rows.push(['ชนิดเครื่องมือ','แผนก','หมายเลขเครื่อง','วันที่ยืม','วันที่ใช้ (วัน)','ผู้ให้ยืม','ผู้ป่วย']);
    var now2=new Date();
    exp.filter(function(i){return i.status==='ACTIVE';}).forEach(function(i){
      var ld=i.loan.LoanDate;var days=ld?Math.max(0,Math.round((now2-new Date(ld))/86400000)):'-';
      rows.push([S(i.eq.name),S(i.loan.DeptName),i.borrow?S(i.borrow.EquipmentNo||'—'):'—',ld||'-',days,i.borrow?S(i.borrow.GiverName||'-'):'-',S(i.loan.PatientName)]);
    });
  }
  return rows;
}
function dlPDF(title,rows){
  var css='body{font-family:Arial,Tahoma,sans-serif;font-size:10px;color:#111;margin:0;padding:14px}h2{font-size:14px;margin-bottom:4px;color:#1a1a2e}.info{font-size:9px;color:#666;margin-bottom:10px}table{border-collapse:collapse;width:100%;font-size:9px}th{background:#1a1a2e;color:#fff;padding:5px 7px;text-align:left;white-space:nowrap}td{border:1px solid #ccc;padding:3px 7px;vertical-align:top}tr:nth-child(even) td{background:#f5f5f5}@media print{button{display:none}}';
  var tHead=rows[0]?'<tr>'+rows[0].map(function(h){return'<th>'+String(h==null?'':h).replace(/</g,'&lt;')+'</th>';}).join('')+'</tr>':'';
  var tBody=rows.slice(1).map(function(r){return'<tr>'+r.map(function(c){return'<td>'+String(c==null?'':c).replace(/</g,'&lt;')+'</td>';}).join('')+'</tr>';}).join('');
  var html='<!DOCTYPE html><html><head><meta charset="UTF-8"><style>'+css+'</style><title>'+title+'</title></head><body>'
    +'<h2>'+title+'</h2>'
    +'<div class="info">ออกรายงาน: '+new Date().toLocaleDateString('th-TH',{year:'numeric',month:'long',day:'numeric',hour:'2-digit',minute:'2-digit'})+' | จำนวน '+(rows.length-1)+' รายการ</div>'
    +'<table><thead>'+tHead+'</thead><tbody>'+tBody+'</tbody></table>'
    +'<br><button onclick="window.print()" style="padding:8px 18px;background:#1a1a2e;color:#fff;border:none;border-radius:4px;cursor:pointer;font-size:12px">🖨️ พิมพ์/บันทึก PDF</button>'
    +'</body></html>';
  var win=window.open('','_blank','width=1100,height=750,scrollbars=yes');
  if(!win){alert('กรุณาอนุญาต popup');return;}
  win.document.write(html);win.document.close();
  setTimeout(function(){try{win.print();}catch(e){}},800);
}
function dlPage(page,fmt){
  if(!loans){toast('ยังไม่มีข้อมูล','warning');return;}
  var rows=getPageData(page);
  if(!rows||rows.length<=1){toast('ไม่มีข้อมูล','warning');return;}
  var FNAME={'rec':'บันทึกยืม-คืน-โอน','all':'ข้อมูลทั้งหมด','daily':'สรุปประจำวัน','sum':'สถิติ','dash':'Dashboard'};
  var TITLE={'rec':'บันทึกยืม-คืน-โอน','all':'ข้อมูลทั้งหมด','daily':'สรุปรายการยืมประจำวัน','sum':'สถิติการใช้งานเครื่องมือแพทย์','dash':'Dashboard เครื่องมือแพทย์'};
  var fname=(FNAME[page]||page)+'_'+new Date().toISOString().split('T')[0];
  var title=TITLE[page]||page;
  if(fmt==='csv'){dlCSV(rows.map(function(r){return r.map(function(c){return'"'+String(c==null?'':c).replace(/"/g,'""')+'"';}).join(',');}).join('\n'),fname+'.csv');}
  else if(fmt==='xlsx'||fmt==='xls'){
    var xml='<?xml version="1.0" encoding="UTF-8"?><?mso-application progid="Excel.Sheet"?><Workbook xmlns="urn:schemas-microsoft-com:office:spreadsheet" xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"><Styles><Style ss:ID="h"><Font ss:Bold="1" ss:Color="#FFFFFF"/><Interior ss:Color="#1a1a2e" ss:Pattern="Solid"/></Style></Styles><Worksheet ss:Name="ข้อมูล"><Table>'+rows.map(function(row,ri){return'<Row>'+row.map(function(cell){var v=String(cell==null?'':cell).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');var isNum=v!==''&&!isNaN(v)&&ri>0;if(ri===0)return'<Cell ss:StyleID="h"><Data ss:Type="String">'+v+'</Data></Cell>';if(isNum)return'<Cell><Data ss:Type="Number">'+v+'</Data></Cell>';return'<Cell><Data ss:Type="String">'+v+'</Data></Cell>';}).join('')+'</Row>';}).join('')+'</Table></Worksheet></Workbook>';
    var blob=new Blob([xml],{type:'application/vnd.ms-excel;charset=utf-8'});
    var a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=fname+(fmt==='xlsx'?'.xlsx':'.xls');a.click();
  }
  else if(fmt==='pdf'){dlPDF(title,rows);}
}
function dlCSV(content,filename){var blob=new Blob(['\uFEFF'+content],{type:'text/csv;charset=utf-8;'});var a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=filename;a.click();}
function showPg(page,el){
  document.querySelectorAll('.pg').forEach(function(p){p.classList.remove('on');});
  document.querySelectorAll('.nt').forEach(function(t){t.classList.remove('on');});
  document.getElementById('pg-'+page).classList.add('on');
  if(el)el.classList.add('on');
  // หยุด timer ถ้าออกจากหน้าสถิติ
  if(page!=='sum'&&window.stopSumRealtime)stopSumRealtime();
  if(loans===null){
    loadAll().then(function(){
      if(page==='sum'){initSumFilters();loadSum().then(function(){startSumRealtime();});}
    });
    return;
  }
  if(page==='dash')updateDash();
  else if(page==='daily')renderDaily();
  else if(page==='sum'){initSumFilters();loadSum().then(function(){startSumRealtime();});}
  else if(page==='rec')renderRec();
  else if(page==='all')renderAll();
  else if(page==='admin'&&_adminUnlocked){renderAdminLoans();renderAdminBorrows();}
}
function openM(id){document.getElementById(id).classList.add('on');}
function closeM(id){document.getElementById(id).classList.remove('on');}
function toast(msg,type){if(!type)type='success';var w=document.getElementById('tw');var t=document.createElement('div');t.className='toast '+type;t.textContent={success:'✅',error:'❌',warning:'⚠️'}[type]+' '+msg;w.appendChild(t);setTimeout(function(){t.remove();},4000);}
function bigToast(msg,type){if(!type)type='success';var overlay=document.createElement('div');overlay.style.cssText='position:fixed;inset:0;background:rgba(0,0,0,.6);z-index:9000;display:flex;align-items:center;justify-content:center;padding:20px';var colors={success:['var(--ok)','rgba(16,185,129,.15)'],error:['var(--er)','rgba(239,68,68,.15)'],warning:['var(--wn)','rgba(245,158,11,.15)']};var ic={success:'✅',error:'❌',warning:'⚠️'};var c=colors[type]||colors.success;var box=document.createElement('div');box.style.cssText='background:var(--s1);border:2px solid '+c[0]+';border-radius:12px;padding:32px 40px;text-align:center;max-width:420px;width:100%';var lines=msg.split('\n');box.innerHTML='<div style="font-size:48px;margin-bottom:14px">'+ic[type]+'</div><div style="font-size:22px;font-weight:700;color:'+c[0]+';margin-bottom:8px;line-height:1.4">'+lines[0]+'</div>'+(lines.slice(1).map(function(l){return'<div style="font-size:15px;color:var(--mu);margin-top:6px">'+l+'</div>';}).join(''))+'<button style="margin-top:22px;background:'+c[0]+';color:'+(type==='warning'?'#000':'#fff')+';border:none;border-radius:7px;padding:10px 28px;font-size:15px;font-weight:700;cursor:pointer;font-family:var(--fn)" onclick="this.closest(\'[data-bt]\').remove()">ตกลง</button>';overlay.setAttribute('data-bt','1');overlay.appendChild(box);overlay.addEventListener('click',function(e){if(e.target===overlay)overlay.remove();});document.body.appendChild(overlay);if(type==='success')setTimeout(function(){if(overlay.parentNode)overlay.remove();},3000);}
</script>
<script>// ═══════════════════════════════════════════════════════════════════
// PATCH v2: สถานะเครื่องมือแพทย์
// - Dashboard: ตารางสถานะ + % คอลัมน์
// - หน้าลงบันทึก: แสดง Standby badge ใน action row
// - Admin: แท็บจัดการสถานะเครื่องมือ
// ═══════════════════════════════════════════════════════════════════
(function(){

// ── CSS เพิ่มเติม ──────────────────────────────────────────────────
var style=document.createElement('style');
style.textContent=[
  '.pct-bar{display:inline-block;height:4px;border-radius:2px;vertical-align:middle;margin-left:4px}',
  '.pct-lbl{font-size:10px;color:var(--mu);font-family:var(--mo)}',
  '.inv-pct{display:flex;align-items:center;gap:4px;justify-content:center}',
  '#invTableCard table td,#invTableCard table th{transition:background .12s}',
  '.standby-rec-badge{display:inline-flex;align-items:center;gap:3px;background:rgba(245,158,11,.15);border:1px solid rgba(245,158,11,.4);border-radius:10px;padding:2px 8px;font-size:10px;font-weight:700;color:#f59e0b;vertical-align:middle;margin-left:4px}'
].join('');
document.head.appendChild(style);

// ── INV DATA ──────────────────────────────────────────────────────
var _INV_DEFAULT=[
  {name:'Ventilator',total:74,inactive:25,forDept:5},
  {name:'Ventilator transfer',total:15,inactive:1,forDept:5},
  {name:'HFNC',total:78,inactive:25,forDept:0},
  {name:'HFNC transfer',total:2,inactive:0,forDept:0},
  {name:'BiPAP',total:5,inactive:2,forDept:0},
  {name:'BIRD',total:12,inactive:0,forDept:1},
  {name:'Infusion pump',total:223,inactive:13,forDept:28},
  {name:'Infusion pump for chemo',total:15,inactive:0,forDept:0},
  {name:'Syringe pump',total:44,inactive:0,forDept:14},
  {name:'PCA',total:5,inactive:0,forDept:0},
  {name:'Enteral feeding pump',total:22,inactive:0,forDept:0},
  {name:'Oxygen flow meter pipeline',total:267,inactive:0,forDept:203},
  {name:'Oxygen flow meter regulator',total:10,inactive:0,forDept:0},
  {name:'Continuous suction pipeline',total:255,inactive:1,forDept:113},
  {name:'Intermittent suction pipeline',total:15,inactive:2,forDept:0},
  {name:'thoracic suction pipeline',total:13,inactive:0,forDept:0},
  {name:'PEEP VALVE',total:4,inactive:0,forDept:0},
  {name:'CPAP VALVE',total:15,inactive:0,forDept:0},
  {name:'Heated probe',total:2,inactive:0,forDept:0}
];
function _invGet(){try{var v=localStorage.getItem('meq_inventory');return v?JSON.parse(v):JSON.parse(JSON.stringify(_INV_DEFAULT));}catch(e){return JSON.parse(JSON.stringify(_INV_DEFAULT));}}
function _invSet(d){try{localStorage.setItem('meq_inventory',JSON.stringify(d));}catch(e){}}
window._EQ_INV=_invGet();

// ── % bar helper ────────────────────────────────────────────────
function pBar(pct,color){
  var w=Math.min(100,Math.max(0,pct));
  return '<div class="inv-pct">'
    +'<div class="pct-bar" style="width:'+Math.round(w*0.4)+'px;background:'+color+'"></div>'
    +'<span class="pct-lbl">'+w.toFixed(1)+'%</span>'
    +'</div>';
}

// ── On Loan from live data ───────────────────────────────────────
function getOnLoanByName(){
  var map={};
  if(!window.expanded)return map;
  (window.expanded()||[]).forEach(function(item){
    if(item.status==='ACTIVE'){var n=(item.eq&&item.eq.name)||'';if(n)map[n]=(map[n]||0)+1;}
  });
  return map;
}

// ── Render dashboard table ───────────────────────────────────────
window.renderInventoryTable=function(){
  var cont=document.getElementById('invTableCont');if(!cont)return;
  var inv=window._EQ_INV;
  var olMap=getOnLoanByName();

  var rows=inv.map(function(eq){
    var T=eq.total||0,I=eq.inactive||0,F=eq.forDept||0;
    var A=Math.max(0,T-I-F);
    var OL=0;
    Object.keys(olMap).forEach(function(k){if(k.trim().toLowerCase()===eq.name.trim().toLowerCase())OL=olMap[k];});
    var AV=Math.max(0,A-OL);

    // % of Total
    var pA  = T>0?A/T*100:0;
    var pI  = T>0?I/T*100:0;
    var pFD = T>0?F/T*100:0;
    // % of Active
    var pOL = A>0?OL/A*100:0;
    var pAV = A>0?AV/A*100:0;

    var avColor=AV>5?'var(--ok)':AV>0?'var(--wn)':'var(--er)';
    var olColor=OL>0?'var(--ac)':'var(--mu)';

    return '<tr onmouseover="this.style.background=\'rgba(255,255,255,.03)\'" onmouseout="this.style.background=\'transparent\'">'
      +'<td style="font-size:12px;font-weight:600;padding:6px 10px;border-bottom:1px solid var(--bd)">'+eq.name+'</td>'
      +'<td style="text-align:center;font-family:var(--mo);padding:6px 8px;border-bottom:1px solid var(--bd)">'+T+'</td>'
      // Active
      +'<td style="text-align:center;padding:6px 8px;border-bottom:1px solid var(--bd)">'
        +'<div style="font-family:var(--mo);font-weight:700;color:var(--ok)">'+A+'</div>'
        +pBar(pA,'var(--ok)')
      +'</td>'
      // Inactive
      +'<td style="text-align:center;padding:6px 8px;border-bottom:1px solid var(--bd)">'
        +'<div style="font-family:var(--mo);font-weight:700;color:var(--er)">'+I+'</div>'
        +pBar(pI,'var(--er)')
      +'</td>'
      // For Dept
      +'<td style="text-align:center;padding:6px 8px;border-bottom:1px solid var(--bd)">'
        +'<div style="font-family:var(--mo);font-weight:700;color:#f59e0b">'+F+'</div>'
        +pBar(pFD,'#f59e0b')
      +'</td>'
      // On Loan
      +'<td style="text-align:center;padding:6px 8px;border-bottom:1px solid var(--bd)">'
        +'<div style="font-family:var(--mo);font-weight:700;color:'+olColor+'">'+OL+'</div>'
        +(A>0?pBar(pOL,'var(--ac)'):'<span class="pct-lbl">—</span>')
      +'</td>'
      // Available
      +'<td style="text-align:center;padding:6px 8px;border-bottom:1px solid var(--bd)">'
        +'<div style="font-family:var(--mo);font-size:13px;font-weight:700;color:'+avColor+'">'+AV+'</div>'
        +(A>0?pBar(pAV,avColor):'<span class="pct-lbl">—</span>')
      +'</td>'
      +'</tr>';
  }).join('');

  // Totals
  var tT=0,tA=0,tI=0,tF=0,tOL=0,tAV=0;
  inv.forEach(function(eq){
    var T=eq.total||0,I=eq.inactive||0,F=eq.forDept||0;
    var A=Math.max(0,T-I-F);
    var OL=0;
    Object.keys(olMap).forEach(function(k){if(k.trim().toLowerCase()===eq.name.trim().toLowerCase())OL=olMap[k];});
    tT+=T;tA+=A;tI+=I;tF+=F;tOL+=OL;tAV+=Math.max(0,A-OL);
  });
  var tpA=tT>0?tA/tT*100:0,tpI=tT>0?tI/tT*100:0,tpF=tT>0?tF/tT*100:0;
  var tpOL=tA>0?tOL/tA*100:0,tpAV=tA>0?tAV/tA*100:0;

  cont.innerHTML=
    '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;flex-wrap:wrap;gap:6px">'
    +'<div style="font-size:11px;color:var(--mu)">🕐 อัปเดตล่าสุด: <span id="invLastUpdate" style="color:var(--ac)">'+new Date().toLocaleString('th-TH')+'</span></div>'
    +'<button class="btn bg sm" onclick="refreshInventoryTable()">🔄 รีเฟรช</button></div>'
    +'<div style="overflow-x:auto"><table style="width:100%;border-collapse:collapse;font-size:12px">'
    +'<thead><tr style="background:var(--s2)">'
    +'<th style="text-align:left;padding:7px 10px;color:var(--mu);font-size:10px;font-weight:700;text-transform:uppercase;border-bottom:2px solid var(--bd)">Medical Equipment</th>'
    +'<th style="text-align:center;padding:7px 8px;color:var(--mu);font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">Total</th>'
    +'<th style="text-align:center;padding:7px 8px;color:var(--ok);font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">Active<br><span style="color:var(--mu);font-weight:400">% of Total</span></th>'
    +'<th style="text-align:center;padding:7px 8px;color:var(--er);font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">Inactive<br><span style="color:var(--mu);font-weight:400">% of Total</span></th>'
    +'<th style="text-align:center;padding:7px 8px;color:#f59e0b;font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">For Dept Use<br><span style="color:var(--mu);font-weight:400">% of Total</span></th>'
    +'<th style="text-align:center;padding:7px 8px;color:var(--ac);font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">On Loan<br><span style="color:var(--mu);font-weight:400">% of Active</span></th>'
    +'<th style="text-align:center;padding:7px 8px;color:var(--ok);font-size:10px;font-weight:700;border-bottom:2px solid var(--bd)">Available<br><span style="color:var(--mu);font-weight:400">% of Active</span></th>'
    +'</tr></thead>'
    +'<tbody>'+rows+'</tbody>'
    +'<tfoot><tr style="background:linear-gradient(135deg,rgba(0,200,255,.08),rgba(124,58,237,.06));border-top:2px solid var(--ac)">'
    +'<td style="padding:8px 10px;font-weight:700;font-size:12px;color:var(--ac)">รวมทั้งหมด</td>'
    +'<td style="text-align:center;font-family:var(--mo);font-weight:700;padding:8px">'+tT+'</td>'
    +'<td style="text-align:center;padding:8px"><div style="font-family:var(--mo);font-weight:700;color:var(--ok)">'+tA+'</div>'+pBar(tpA,'var(--ok)')+'</td>'
    +'<td style="text-align:center;padding:8px"><div style="font-family:var(--mo);font-weight:700;color:var(--er)">'+tI+'</div>'+pBar(tpI,'var(--er)')+'</td>'
    +'<td style="text-align:center;padding:8px"><div style="font-family:var(--mo);font-weight:700;color:#f59e0b">'+tF+'</div>'+pBar(tpF,'#f59e0b')+'</td>'
    +'<td style="text-align:center;padding:8px"><div style="font-family:var(--mo);font-weight:700;color:var(--ac)">'+tOL+'</div>'+pBar(tpOL,'var(--ac)')+'</td>'
    +'<td style="text-align:center;padding:8px"><div style="font-family:var(--mo);font-weight:700;font-size:14px;color:var(--ok)">'+tAV+'</div>'+pBar(tpAV,'var(--ok)')+'</td>'
    +'</tr></tfoot></table></div>'
    +'<div style="display:flex;flex-wrap:wrap;gap:12px;margin-top:10px;padding:8px 12px;background:var(--s2);border-radius:6px;font-size:11px;color:var(--mu)">'
    +'<span>🟢 <b style="color:var(--ok)">Active</b> = Total−Inactive−ForDept &nbsp;|&nbsp; % of Total</span>'
    +'<span>🔴 <b style="color:var(--er)">Inactive</b> % of Total</span>'
    +'<span>🟡 <b style="color:#f59e0b">For Dept</b> % of Total</span>'
    +'<span>🔵 <b style="color:var(--ac)">On Loan</b> = ข้อมูลยืมจริง &nbsp;|&nbsp; % of Active</span>'
    +'<span>✅ <b style="color:var(--ok)">Available</b> = Active−OnLoan &nbsp;|&nbsp; % of Active</span>'
    +'</div>';
};

window.refreshInventoryTable=function(){
  window._EQ_INV=_invGet();renderInventoryTable();
  var el=document.getElementById('invLastUpdate');
  if(el)el.textContent=new Date().toLocaleString('th-TH');
  if(window.toast)window.toast('รีเฟรชสถานะเครื่องมือแล้ว','success');
};

setInterval(function(){
  var d=document.getElementById('pg-dash');
  if(d&&d.classList.contains('on'))renderInventoryTable();
},5*60*1000);

// ── Patch updateDash ─────────────────────────────────────────────
var _origUD=window.updateDash;
window.updateDash=function(){
  if(_origUD)_origUD.apply(this,arguments);
  setTimeout(function(){
    var dc=document.getElementById('dashCont');if(!dc)return;
    if(!document.getElementById('invTableCard')){
      var card=document.createElement('div');
      card.id='invTableCard';card.className='card';
      card.innerHTML='<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">'
        +'<div class="ctitle" style="margin:0">📋 สถานะเครื่องมือแพทย์</div>'
        +'<button class="btn bp sm" onclick="refreshInventoryTable()">🔄 รีเฟรช</button></div>'
        +'<div id="invTableCont"><div class="lding"><div class="spin"></div></div></div>';
      dc.appendChild(card);
    }
    renderInventoryTable();
  },300);
};

// ════════════════════════════════════════════════════════════════
// หน้าลงบันทึก: เพิ่ม Standby badge ใน record row ที่มีชื่อ Standby
// patch renderRec เพื่อเพิ่ม cancel button สำหรับ ACTIVE item
// ════════════════════════════════════════════════════════════════
var _origRenderRec=window.renderRec;
window.renderRec=function(){
  if(_origRenderRec)_origRenderRec.apply(this,arguments);
  // หลัง render เสร็จ inject Standby badge เข้าไปใน rows ที่ยังไม่มี
  setTimeout(function(){
    var rows=document.querySelectorAll('#recBody tr.item-row');
    rows.forEach(function(row){
      // ดู text ใน td ที่ 2 (ชื่อเครื่องมือ)
      var cells=row.querySelectorAll('td');
      if(cells.length<2)return;
      var nameCell=cells[1];
      var txt=nameCell.textContent||'';
      if(txt.toLowerCase().includes('standby')&&!nameCell.querySelector('.standby-rec-badge')){
        var badge=document.createElement('span');
        badge.className='standby-rec-badge';
        badge.innerHTML='⏸ STANDBY';
        // หา strong tag ชื่อเครื่อง
        var strong=nameCell.querySelector('strong');
        if(strong)strong.parentNode.insertBefore(badge,strong.nextSibling);
        else nameCell.appendChild(badge);
      }
      // เพิ่ม cancel button สำหรับ ACTIVE items
      var actionCell=cells[cells.length-1];
      var hasCancel=actionCell.querySelector('[data-act="cancelItem"]');
      if(!hasCancel){
        var borrowBtn=actionCell.querySelector('[data-act="borrow"]');
        if(borrowBtn){
          // มี borrow = PENDING → เพิ่มปุ่มยกเลิกรายการ
          var key=borrowBtn.getAttribute('data-key');
          var cancelBtn=document.createElement('button');
          cancelBtn.className='abt';
          cancelBtn.setAttribute('data-act','cancelItem');
          cancelBtn.setAttribute('data-key',key);
          cancelBtn.style.cssText='border-color:var(--er);color:var(--er);font-size:11px;margin-left:4px';
          cancelBtn.textContent='✕ ยกเลิก';
          cancelBtn.onclick=function(){
            var k=this.getAttribute('data-key');
            cancelRecordItem(k);
          };
          actionCell.appendChild(cancelBtn);
        }
      }
    });
  },100);
};

// ── ยกเลิกรายการ PENDING ────────────────────────────────────────
window.cancelRecordItem=async function(key){
  if(!window._rMap)return;
  var item=window._rMap[key];
  if(!item)return;
  var eqName=item.eq?item.eq.name:'รายการนี้';
  if(!confirm('ยกเลิกรายการ "'+eqName+'" ใน '+item.loan.LoanID+' ?'))return;
  try{
    // ถ้า loan มีแค่ item เดียว → cancel loan ทั้งใบ
    var eqList=item.loan.EquipmentList||[];
    if(eqList.length<=1){
      await window.API.cancelLoan(item.loan.LoanID);
      if(window.toast)window.toast('ยกเลิกใบยืม '+item.loan.LoanID+' แล้ว','success');
    } else {
      // mark ใน equipment_list ว่า cancelled
      var newList=eqList.map(function(e,i){
        return i===item.idx?Object.assign({},e,{cancelled:true}):e;
      });
      await window.API.updateLoan(item.loan.LoanID,{equipment_list:newList});
      if(window.toast)window.toast('ยกเลิกรายการ "'+eqName+'" แล้ว','success');
    }
    if(window.loadAll)window.loadAll();
  }catch(e){
    if(window.toast)window.toast('ยกเลิกไม่สำเร็จ: '+e.message,'error');
  }
};

// ════════════════════════════════════════════════════════════════
// ADMIN: Equipment Inventory Manager tab
// ════════════════════════════════════════════════════════════════
function buildInvRows(inv){
  return inv.map(function(eq,i){
    return '<tr onmouseover="this.style.background=\'rgba(245,158,11,.04)\'" onmouseout="this.style.background=\'transparent\'">'
      +'<td style="padding:7px 10px;border-bottom:1px solid var(--bd)">'
        +'<input class="inline-edit" style="width:220px;font-size:12px" value="'+eq.name.replace(/"/g,'&quot;')+'" onchange="invUpdateName('+i+',this.value)">'
      +'</td>'
      +'<td style="padding:7px 10px;border-bottom:1px solid var(--bd);text-align:center">'
        +'<input type="number" min="0" class="inline-edit" style="width:72px;text-align:center" value="'+(eq.total||0)+'" onchange="invUpdate('+i+',\'total\',this.value)">'
      +'</td>'
      +'<td style="padding:7px 10px;border-bottom:1px solid var(--bd);text-align:center">'
        +'<input type="number" min="0" class="inline-edit" style="width:72px;text-align:center;border-color:var(--er)" value="'+(eq.inactive||0)+'" onchange="invUpdate('+i+',\'inactive\',this.value)">'
      +'</td>'
      +'<td style="padding:7px 10px;border-bottom:1px solid var(--bd);text-align:center">'
        +'<input type="number" min="0" class="inline-edit" style="width:72px;text-align:center;border-color:#f59e0b" value="'+(eq.forDept||0)+'" onchange="invUpdate('+i+',\'forDept\',this.value)">'
      +'</td>'
      +'<td style="padding:7px 10px;border-bottom:1px solid var(--bd);text-align:center">'
        +'<button class="abt" style="font-size:11px;border-color:var(--er);color:var(--er)" onclick="invRemove('+i+')">✕ ลบ</button>'
      +'</td></tr>';
  }).join('');
}

function buildInvAdminHTML(){
  var inv=_invGet();
  return '<div class="card">'
    +'<div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;margin-bottom:12px">'
    +'<div class="ctitle" style="margin:0;color:#f59e0b">📊 จัดการสถานะเครื่องมือแพทย์</div>'
    +'<div style="display:flex;gap:6px;flex-wrap:wrap">'
    +'<button class="btn bg sm" onclick="invReload()">🔄 โหลด</button>'
    +'<button class="btn badm sm" onclick="invSave()">✅ บันทึก</button></div></div>'
    +'<div style="background:rgba(245,158,11,.06);border:1px solid rgba(245,158,11,.2);border-radius:6px;padding:8px 12px;margin-bottom:12px;font-size:11px;color:var(--mu)">'
    +'Active = Total−Inactive−ForDept &nbsp;|&nbsp; On Loan = อัตโนมัติ &nbsp;|&nbsp; Available = Active−OnLoan &nbsp;|&nbsp; % คำนวณอัตโนมัติ'
    +'</div>'
    +'<div style="overflow-x:auto"><table style="width:100%;border-collapse:collapse">'
    +'<thead><tr style="background:var(--s2)">'
    +'<th style="text-align:left;padding:8px 10px;font-size:10px;color:var(--mu);font-weight:700;text-transform:uppercase;border-bottom:2px solid var(--bd)">ชื่อเครื่องมือ</th>'
    +'<th style="text-align:center;padding:8px 10px;font-size:10px;font-weight:700;text-transform:uppercase;border-bottom:2px solid var(--bd)">Total ✏️</th>'
    +'<th style="text-align:center;padding:8px 10px;font-size:10px;color:var(--er);font-weight:700;text-transform:uppercase;border-bottom:2px solid var(--bd)">Inactive ✏️</th>'
    +'<th style="text-align:center;padding:8px 10px;font-size:10px;color:#f59e0b;font-weight:700;text-transform:uppercase;border-bottom:2px solid var(--bd)">For Dept Use ✏️</th>'
    +'<th style="text-align:center;padding:8px 10px;font-size:10px;color:var(--mu);font-weight:700;border-bottom:2px solid var(--bd)">จัดการ</th>'
    +'</tr></thead>'
    +'<tbody id="invAdminBody">'+buildInvRows(inv)+'</tbody></table></div>'
    +'<div style="margin-top:14px;padding:14px;background:var(--s2);border-radius:8px">'
    +'<div style="font-size:12px;font-weight:700;color:var(--tx);margin-bottom:10px">➕ เพิ่มรายการเครื่องมือใหม่</div>'
    +'<div style="display:flex;gap:8px;flex-wrap:wrap;align-items:flex-end">'
    +'<div class="fg"><label>ชื่อเครื่องมือ *</label><input type="text" id="newInvName" placeholder="เช่น New Device" style="min-width:180px"></div>'
    +'<div class="fg"><label>Total</label><input type="number" min="0" id="newInvTotal" value="0" style="width:80px;text-align:center"></div>'
    +'<div class="fg"><label>Inactive</label><input type="number" min="0" id="newInvInactive" value="0" style="width:80px;text-align:center"></div>'
    +'<div class="fg"><label>For Dept Use</label><input type="number" min="0" id="newInvForDept" value="0" style="width:80px;text-align:center"></div>'
    +'<button class="btn badm" style="margin-bottom:3px" onclick="invAdd()">➕ เพิ่ม</button>'
    +'</div></div></div>';
}

window.invUpdate=function(i,field,val){var inv=_invGet();if(!inv[i])return;inv[i][field]=parseInt(val,10)||0;_invSet(inv);window._EQ_INV=inv;};
window.invUpdateName=function(i,val){var inv=_invGet();if(!inv[i])return;inv[i].name=(val||'').trim();_invSet(inv);window._EQ_INV=inv;};
window.invAdd=function(){
  var name=(document.getElementById('newInvName').value||'').trim();
  if(!name){if(window.toast)window.toast('กรุณาระบุชื่อเครื่องมือ','error');return;}
  var inv=_invGet();
  inv.push({name:name,total:parseInt(document.getElementById('newInvTotal').value,10)||0,inactive:parseInt(document.getElementById('newInvInactive').value,10)||0,forDept:parseInt(document.getElementById('newInvForDept').value,10)||0});
  _invSet(inv);window._EQ_INV=inv;
  document.getElementById('newInvName').value='';
  document.getElementById('newInvTotal').value='0';
  document.getElementById('newInvInactive').value='0';
  document.getElementById('newInvForDept').value='0';
  var b=document.getElementById('invAdminBody');if(b)b.innerHTML=buildInvRows(_invGet());
  if(window.toast)window.toast('เพิ่มรายการสำเร็จ','success');
};
window.invRemove=function(i){
  var inv=_invGet();if(!inv[i])return;
  if(!confirm('ลบ "'+inv[i].name+'" ?'))return;
  inv.splice(i,1);_invSet(inv);window._EQ_INV=inv;invReload();
};
window.invSave=function(){_invSet(_invGet());window._EQ_INV=_invGet();renderInventoryTable();if(window.toast)window.toast('บันทึกสำเร็จ!','success');};
window.invReload=function(){window._EQ_INV=_invGet();var p=document.getElementById('adm-inv-mgr');if(p)p.innerHTML=buildInvAdminHTML();};

function injectInvAdminTab(){
  var ac=document.getElementById('adminContent');if(!ac||document.getElementById('atab-inv'))return;
  var tabs=ac.querySelector('.admin-tabs');
  if(tabs){
    var btn=document.createElement('button');
    btn.className='atab';btn.id='atab-inv';
    btn.setAttribute('onclick','showAdminTab(\'inv-mgr\',this)');
    btn.innerHTML='📊 จัดการสถานะเครื่องมือ';
    tabs.appendChild(btn);
  }
  var panel=document.createElement('div');
  panel.className='admin-sub';panel.id='adm-inv-mgr';
  panel.innerHTML=buildInvAdminHTML();
  ac.appendChild(panel);
}

var _origSAT=window.showAdminTab;
window.showAdminTab=function(id,el){
  if(id==='inv-mgr'){
    document.querySelectorAll('.admin-sub').forEach(function(s){s.classList.remove('on');});
    document.querySelectorAll('.atab').forEach(function(t){t.classList.remove('on');});
    var p=document.getElementById('adm-inv-mgr');if(p)p.classList.add('on');
    if(el)el.classList.add('on');invReload();
  } else {if(_origSAT)_origSAT.apply(this,arguments);}
};

var _origCAP=window.checkAdminPin;
window.checkAdminPin=function(){
  if(_origCAP)_origCAP.apply(this,arguments);
  setTimeout(function(){var ac=document.getElementById('adminContent');if(ac&&ac.style.display!=='none')injectInvAdminTab();},400);
};
new MutationObserver(function(){
  var ac=document.getElementById('adminContent');
  if(ac&&ac.style.display!=='none'&&!document.getElementById('atab-inv'))injectInvAdminTab();
}).observe(document.body,{childList:true,subtree:true,attributes:true,attributeFilter:['style']});

})();
// ══════════════════════════════════════════
// MANUAL PAGE
// ══════════════════════════════════════════
var _DOCX_B64="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";

// Override showPg to handle 'manual'
(function(){
  var _orig = window.showPg;
  window.showPg = function(page, el){
    if(page === 'manual'){
      document.querySelectorAll('.pg').forEach(function(p){p.classList.remove('on');});
      document.querySelectorAll('.nt').forEach(function(t){t.classList.remove('on');});
      var pg = document.getElementById('pg-manual');
      if(pg) pg.classList.add('on');
      if(el) el.classList.add('on');
      if(window.stopSumRealtime) stopSumRealtime();
      return;
    }
    _orig.apply(this, arguments);
  };
})();

function showMTab(name, el){
  ['info','word'].forEach(function(t){
    var btn = document.getElementById('mtab-'+t);
    var cnt = document.getElementById('mcont-'+t);
    if(btn){ btn.style.background=''; btn.style.color='var(--mu)'; }
    if(cnt) cnt.style.display='none';
  });
  var ebtn = document.getElementById('mtab-'+name);
  var ecnt = document.getElementById('mcont-'+name);
  if(ebtn){ ebtn.style.background='rgba(0,200,255,.12)'; ebtn.style.color='var(--ac)'; }
  if(ecnt) ecnt.style.display='block';
  if(name === 'word') _initWordViewer();
}

var _wordReady = false;
function _initWordViewer(){
  if(_wordReady) return;
  var status = document.getElementById('wv-status');
  var body   = document.getElementById('wv-body');
  if(!status||!body) return;

  function renderDocx(){
    _wordReady = true;
    var bin = atob(_DOCX_B64);
    var arr = new Uint8Array(bin.length);
    for(var i=0;i<bin.length;i++) arr[i] = bin.charCodeAt(i);
    mammoth.convertToHtml({arrayBuffer: arr.buffer},{
      styleMap:[
        "p[style-name='Heading 1'] => h1",
        "p[style-name='Heading 2'] => h2",
        "p[style-name='Heading 3'] => h3"
      ]
    }).then(function(r){
      body.innerHTML = r.value || '<p style="color:var(--mu)">ไม่พบเนื้อหา</p>';
      // style headings inline
      body.querySelectorAll('h1').forEach(function(h){
        h.style.cssText='font-size:19px;font-weight:700;color:var(--ac);margin:22px 0 8px;padding-bottom:6px;border-bottom:1px solid var(--bd)';
      });
      body.querySelectorAll('h2').forEach(function(h){
        h.style.cssText='font-size:15px;font-weight:700;color:var(--tx);margin:16px 0 5px';
      });
      body.querySelectorAll('h3').forEach(function(h){
        h.style.cssText='font-size:13px;font-weight:700;color:var(--mu);margin:12px 0 4px';
      });
      body.querySelectorAll('table').forEach(function(t){
        t.style.cssText='width:100%;border-collapse:collapse;font-size:13px;margin:10px 0';
      });
      body.querySelectorAll('th').forEach(function(t){
        t.style.cssText='background:var(--s2);color:var(--mu);font-size:11px;text-transform:uppercase;letter-spacing:.4px;padding:6px 10px;border:1px solid var(--bd)';
      });
      body.querySelectorAll('td').forEach(function(t){
        t.style.cssText='padding:6px 10px;border:1px solid var(--bd);vertical-align:top';
      });
      body.querySelectorAll('p').forEach(function(p){p.style.marginBottom='7px';});
      status.style.display = 'none';
      body.style.display   = 'block';
    }).catch(function(e){
      status.innerHTML = '<span style="color:var(--mu)">โหลดไม่สำเร็จ: '+e.message+'<br>กดปุ่ม "📄 Word" เพื่อดาวน์โหลดแทน</span>';
    });
  }

  if(window.mammoth){ renderDocx(); }
  else {
    var s = document.createElement('script');
    s.src = 'https://cdnjs.cloudflare.com/ajax/libs/mammoth/1.6.0/mammoth.browser.min.js';
    s.onload  = renderDocx;
    s.onerror = function(){
      status.innerHTML = '<span style="color:var(--mu)">ไม่สามารถโหลด word viewer<br>กดปุ่ม "📄 Word" เพื่อดาวน์โหลด</span>';
    };
    document.head.appendChild(s);
  }
}

function _b64Blob(b64, mime){
  var bin=atob(b64), arr=new Uint8Array(bin.length);
  for(var i=0;i<bin.length;i++) arr[i]=bin.charCodeAt(i);
  return new Blob([arr],{type:mime});
}
function _save(blob, name){
  var a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=name; a.click();
  setTimeout(function(){URL.revokeObjectURL(a.href);},3000);
}

function manDL(fmt){
  var fname = 'คู่มือระบบยืมคืนเครื่องมือแพทย์';

  // ── Word ──────────────────────────────────────────────────
  if(fmt==='word'){
    _save(_b64Blob(_DOCX_B64,'application/vnd.openxmlformats-officedocument.wordprocessingml.document'), fname+'.docx');
    return;
  }

  // ── PDF ───────────────────────────────────────────────────
  if(fmt==='pdf'){
    var pw = window.open('','_blank','width=960,height=720');
    if(!pw){alert('กรุณาอนุญาต popup สำหรับ PDF');return;}
    var host = document.getElementById('infographic-host');
    var src  = host ? host.innerHTML : '';
    // collect infographic CSS from document
    var css = '';
    Array.from(document.styleSheets).forEach(function(ss){
      try{
        Array.from(ss.cssRules||[]).forEach(function(r){
          if(r.selectorText && r.selectorText.indexOf('#infographic-host')>-1){
            css += r.cssText+'\n';
          }
        });
      }catch(e){}
    });
    // strip scope prefix for standalone
    css = css.replace(/#infographic-host\s*/g,'');
    pw.document.write('<!DOCTYPE html><html lang="th"><head><meta charset="UTF-8">'
      +'<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700;800&family=IBM+Plex+Mono:wght@400;600&display=swap" rel="stylesheet">'
      +'<style>:root{--bg:#0a0f1e;--bg2:#0f1629;--card:#131d35;--card2:#18243e;--bd:rgba(255,255,255,.08);--ac1:#00d4ff;--ac2:#7c5cfc;--ac3:#00e5a0;--ac4:#ff6b35;--ac5:#ffd166;--tx:#e8edf8;--mu:#8892aa;--fn:"Sarabun",sans-serif;--mo:"IBM Plex Mono",monospace}*{box-sizing:border-box;margin:0;padding:0}body{font-family:var(--fn);background:var(--bg);color:var(--tx)}'
      +css
      +'@media print{@page{size:A4;margin:10mm}body{-webkit-print-color-adjust:exact;print-color-adjust:exact}.no-print{display:none}}'
      +'</style></head><body>'+src
      +'<div class="no-print" style="position:fixed;bottom:16px;right:16px">'
      +'<button onclick="window.print()" style="padding:10px 20px;background:#00d4ff;color:#000;border:none;border-radius:6px;font-size:14px;font-weight:700;cursor:pointer;font-family:Sarabun,sans-serif">🖨️ พิมพ์ / บันทึก PDF</button>'
      +'</div></body></html>');
    pw.document.close();
    return;
  }

  // ── HTML ──────────────────────────────────────────────────
  if(fmt==='html'){
    var host = document.getElementById('infographic-host');
    var src  = host ? host.innerHTML : '';
    var css  = '';
    Array.from(document.styleSheets).forEach(function(ss){
      try{
        Array.from(ss.cssRules||[]).forEach(function(r){
          if(r.selectorText && r.selectorText.indexOf('#infographic-host')>-1)
            css += r.cssText+'\n';
        });
      }catch(e){}
    });
    css = css.replace(/#infographic-host\s*/g,'');
    var full = '<!DOCTYPE html><html lang="th"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1.0">'
      +'<title>'+fname+'</title>'
      +'<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700;800&family=IBM+Plex+Mono:wght@400;600&display=swap" rel="stylesheet">'
      +'<style>:root{--bg:#0a0f1e;--bg2:#0f1629;--card:#131d35;--card2:#18243e;--bd:rgba(255,255,255,.08);--ac1:#00d4ff;--ac2:#7c5cfc;--ac3:#00e5a0;--ac4:#ff6b35;--ac5:#ffd166;--tx:#e8edf8;--mu:#8892aa;--fn:"Sarabun",sans-serif;--mo:"IBM Plex Mono",monospace}*{box-sizing:border-box;margin:0;padding:0}body{font-family:var(--fn);background:var(--bg);color:var(--tx)}'+css+'</style>'
      +'</head><body>'+src+'</body></html>';
    _save(new Blob([full],{type:'text/html;charset=utf-8'}), fname+'-infographic.html');
    return;
  }

  // ── CSV / XLS / XLSX ──────────────────────────────────────
  var rows=[
    ['หมวด','หัวข้อ','รายละเอียด','หมายเหตุ'],
    ['ผู้ใช้งาน','แผนกผู้ยืม','กรอกแบบฟอร์มแจ้งยืมเครื่องมือให้ผู้ป่วย','ไม่ต้องมีรหัสผ่าน'],
    ['ผู้ใช้งาน','จนท.บริการเครื่องมือฯ','บันทึกการจ่าย คืน และโอนเครื่องมือ','ต้องมีรหัสผ่าน'],
    ['ผู้ใช้งาน','Admin','จัดการข้อมูลทั้งหมด แก้ไข เพิ่ม ลบ ตั้งค่า','ต้องมีรหัส Admin'],
    ['เมนู','📋 ลงบันทึกแจ้งยืม','กรอกข้อมูลผู้ยืม เลือกเครื่องมือ ส่งแบบฟอร์ม','สำหรับแผนกผู้ยืม'],
    ['เมนู','📝 ลงบันทึกยืม-คืน-โอน','จัดการรายการ บันทึกจ่าย รับคืน โอนเครื่องมือ','ต้องรหัสผ่าน'],
    ['เมนู','📅 สรุปรายการประจำวัน','ดูรายการกำลังยืมอยู่ทั้งหมด Real-time','อัปเดตทุก 5 นาที'],
    ['เมนู','📊 Dashboard','ภาพรวม สถิติ แจ้งเตือน HFNC','Real-time'],
    ['เมนู','🗂 ข้อมูลทั้งหมด','ค้นหาประวัติการยืมทุกรายการในระบบ',''],
    ['เมนู','📈 สถิติ','กราฟ ค่าเฉลี่ย ยืมเกิน 30 วัน Circuit HFNC set',''],
    ['เมนู','⚙️ Admin','จัดการข้อมูลและตั้งค่าระบบ','ต้องรหัส Admin'],
    ['ขั้นตอน','1. แจ้งยืม','กรอกแบบฟอร์มใน แท็บ 📋 → เลือกเครื่องมือ → กด ส่งแบบฟอร์ม → ได้ LoanID','แผนกผู้ยืม'],
    ['ขั้นตอน','2. บันทึกจ่าย','แท็บ 📝 → กด บันทึก → เลือกผู้ให้ยืม หมายเลขเครื่อง อุปกรณ์ → บันทึก','จนท.'],
    ['ขั้นตอน','3. โอน (ถ้ามี)','กด 🔄 โอน → เลือกผู้โอน แผนกปลายทาง → โอน','จนท.'],
    ['ขั้นตอน','4. บันทึกคืน','กด ↩ คืน → ติ๊กตัวเครื่อง+อุปกรณ์ → เลือกผู้รับคืน เหตุผล → บันทึก','จนท.'],
    ['ขั้นตอน','5. คืนอุปกรณ์ค้าง','กด 📦 คืนอุปกรณ์ → ติ๊กรายการที่คืน → บันทึก','จนท.'],
    ['สถานะ','รอดำเนินการจ่าย (สีเหลือง)','ส่งแบบฟอร์มแล้ว รอ จนท.จัดการ','→ จนท.กดบันทึก'],
    ['สถานะ','ยืม (สีเขียว)','กำลังใช้งานอยู่','→ นำมาคืนเมื่อเสร็จ'],
    ['สถานะ','คืน-อุปกรณ์ค้าง (สีส้ม)','คืนตัวเครื่องแล้ว อุปกรณ์ยังค้าง','→ กด 📦 คืนอุปกรณ์'],
    ['สถานะ','คืน (สีเทา)','คืนครบทุกรายการเรียบร้อย','เสร็จสิ้น'],
    ['สถานะ','ยกเลิก (สีแดง)','รายการถูกยกเลิก','สร้างใหม่ถ้าต้องการ'],
    ['HFNC','เกณฑ์อาคาร 100 ปี','เกินเกณฑ์เมื่อ > 30 เครื่อง','Dashboard แสดงสีแดง'],
    ['HFNC','เกณฑ์รวมทุกอาคาร','เกินเกณฑ์เมื่อ > 65 เครื่อง','Dashboard แสดงสีแดง'],
    ['HFNC','HFNC Set Airvo2','หมายเลข 1-25, 36-38, 59-70',''],
    ['HFNC','HFNC Set Heyer','หมายเลข 26-35, 39-58',''],
    ['HFNC','HFNC Set O2FLO','O2FLO1, O2FLO2, O2FLO3',''],
    ['HFNC','HFNC Set MEK','MEK1-MEK5',''],
    ['เคล็ดลับ','ค้นหาเร็ว','ใช้หมายเลขเครื่อง (เช่น MEK1) หรือ LoanID',''],
    ['เคล็ดลับ','ข้อมูลไม่อัปเดต','กด 🔄 โหลด — อัปเดตอัตโนมัติทุก 5 นาที',''],
    ['เคล็ดลับ','ดาวน์โหลดรายงาน','ทุกหน้ารองรับ CSV / XLS / XLSX / PDF','กรอง Filter ก่อนดาวน์โหลด'],
    ['เคล็ดลับ','แก้ไขข้อมูลผิด','Admin → ⚡ แก้ไขด่วน',''],
    ['Admin','ใบยืม & บันทึกยืม','แก้ไข เพิ่ม ลบ เปลี่ยนสถานะ Bulk select',''],
    ['Admin','แก้ไขด่วน','เปลี่ยนสถานะ วันที่ แผนก หมายเลขเครื่อง',''],
    ['Admin','จัดการรายการ','แผนก บุคลากร เครื่องมือ อุปกรณ์ หมายเลข เหตุผล HFNC set',''],
    ['Admin','สถานะเครื่องมือ','ตั้งค่า Total / Inactive / For Dept Use',''],
  ];

  if(fmt==='csv'){
    var csv = '\uFEFF' + rows.map(function(r){
      return r.map(function(c){return '"'+String(c).replace(/"/g,'""')+'"';}).join(',');
    }).join('\r\n');
    _save(new Blob([csv],{type:'text/csv;charset=utf-8'}), fname+'.csv');
    return;
  }

  // XLS / XLSX (SpreadsheetML)
  var ext = fmt==='xlsx' ? '.xlsx' : '.xls';
  var mime = 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet';
  var xml = '<?xml version="1.0" encoding="UTF-8"?>'
    +'<?mso-application progid="Excel.Sheet"?>'
    +'<Workbook xmlns="urn:schemas-microsoft-com:office:spreadsheet"'
    +' xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet">'
    +'<Styles>'
    +'<Style ss:ID="hdr"><Font ss:Bold="1" ss:Color="#FFFFFF" ss:Size="11"/><Interior ss:Color="#1E5FA8" ss:Pattern="Solid"/><Alignment ss:Horizontal="Center"/></Style>'
    +'<Style ss:ID="alt"><Interior ss:Color="#F7FAFC" ss:Pattern="Solid"/></Style>'
    +'</Styles>'
    +'<Worksheet ss:Name="คู่มือระบบยืมคืนเครื่องมือแพทย์">'
    +'<Table ss:DefaultColumnWidth="120">'
    +'<Column ss:Width="80"/><Column ss:Width="160"/><Column ss:Width="300"/><Column ss:Width="140"/>'
    + rows.map(function(row,ri){
      return '<Row>'
        + row.map(function(cell){
          var v = String(cell).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
          var sty = ri===0 ? ' ss:StyleID="hdr"' : (ri%2===0 ? ' ss:StyleID="alt"' : '');
          return '<Cell'+sty+'><Data ss:Type="String">'+v+'</Data></Cell>';
        }).join('')
        + '</Row>';
    }).join('')
    +'</Table></Worksheet></Workbook>';

  _save(new Blob([xml],{type:mime}), fname+ext);
}


// ──────────────────────────────────────────────────────────────
// PIN MANAGER
// ──────────────────────────────────────────────────────────────
// Load saved PINs from localStorage on init
(function(){
  var savedApp=localStorage.getItem('meq_app_pin');
  var savedAdm=localStorage.getItem('meq_adm_pin');
  if(savedApp)APP_PIN=savedApp;
  if(savedAdm)ADMIN_PIN=savedAdm;
})();

function togglePinShow(inputId,btn){
  var inp=document.getElementById(inputId);
  if(!inp)return;
  if(inp.type==='password'){inp.type='text';btn.textContent='🙈';}
  else{inp.type='password';btn.textContent='👁';}
}

function changePIN(type){
  var newInp,cfmInp,msgEl,saveKey,varName;
  if(type==='app'){
    newInp=document.getElementById('newAppPin');
    cfmInp=document.getElementById('newAppPinConfirm');
    msgEl=document.getElementById('appPinMsg');
    saveKey='meq_app_pin';
  } else {
    newInp=document.getElementById('newAdmPin');
    cfmInp=document.getElementById('newAdmPinConfirm');
    msgEl=document.getElementById('admPinMsg');
    saveKey='meq_adm_pin';
  }
  if(!newInp||!cfmInp||!msgEl)return;
  var newVal=newInp.value.trim();
  var cfmVal=cfmInp.value.trim();

  // validate
  if(!newVal){msgEl.innerHTML='<span style="color:var(--er)">กรุณากรอกรหัสใหม่</span>';return;}
  if(newVal.length<4){msgEl.innerHTML='<span style="color:var(--er)">รหัสต้องมีอย่างน้อย 4 ตัวอักษร</span>';return;}
  if(newVal!==cfmVal){msgEl.innerHTML='<span style="color:var(--er)">รหัสยืนยันไม่ตรงกัน</span>';return;}
  if(!confirm('ยืนยันการเปลี่ยนรหัส'+(type==='app'?' App PIN':' Admin PIN')+' เป็น "'+newVal+'" ?'))return;

  // save
  if(type==='app') APP_PIN=newVal;
  else ADMIN_PIN=newVal;
  localStorage.setItem(saveKey,newVal);

  // update display
  var showInp=document.getElementById(type==='app'?'showAppPin':'showAdmPin');
  if(showInp)showInp.value=newVal;
  newInp.value='';cfmInp.value='';
  msgEl.innerHTML='<span style="color:var(--ok)">✅ เปลี่ยนรหัสสำเร็จ!</span>';
  setTimeout(function(){if(msgEl)msgEl.innerHTML='';},3000);

  // reset unlock state if admin pin changed
  if(type==='app')_pinUnlocked=false;
  toast('เปลี่ยนรหัส'+(type==='app'?' App PIN':' Admin PIN')+' สำเร็จ','success');
}

</script>
</body>
</html>
