
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chingari Creation Billing</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:#f3f5f9;color:#202938}
header{background:#111827;color:white;padding:18px}
header h1{font-size:22px;margin:0 0 5px}
.wrap{max-width:1100px;margin:auto;padding:14px}
.nav,.actions{display:flex;gap:8px;flex-wrap:wrap;margin:12px 0}
button{border:0;border-radius:8px;padding:11px 13px;background:#111827;color:white;font-weight:bold}
button.active,.primary{background:#2563eb}
.danger{background:#dc2626}
.card,.stat{background:white;padding:16px;border-radius:12px;box-shadow:0 2px 8px #0001;margin-bottom:14px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(145px,1fr));gap:12px}
.stat strong{display:block;font-size:24px;margin-top:8px}
.row{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px}
label{display:block;font-size:13px;font-weight:bold;margin:9px 0 5px}
input,select{width:100%;padding:11px;border:1px solid #cbd5e1;border-radius:8px;background:white}
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;min-width:500px}
th,td{text-align:left;padding:10px;border-bottom:1px solid #ddd}
th{background:#f8fafc}
.hidden{display:none}
.total{text-align:right;font-size:20px;margin-top:15px}
small,.muted{color:#64748b}
@media print{header,.nav,.no-print,button{display:none!important}.hidden{display:none!important}.wrap{padding:0}.card{box-shadow:none}}
</style>
</head>
<body>
<header>
<h1>Chingari Creation Billing</h1>
<p>Billing • Products • Customers • Sales</p>
</header>

<div class="wrap">
<div class="nav no-print">
<button class="active" data-tab="dashboard">Dashboard</button>
<button data-tab="billing">Create Bill</button>
<button data-tab="products">Products</button>
<button data-tab="customers">Customers</button>
<button data-tab="sales">Sales History</button>
<button data-tab="backup">Backup</button>
</div>

<section id="dashboard">
<div class="grid">
<div class="stat">Total Sales<strong id="statSales">₹0</strong></div>
<div class="stat">Total Bills<strong id="statBills">0</strong></div>
<div class="stat">Products<strong id="statProducts">0</strong></div>
<div class="stat">Customers<strong id="statCustomers">0</strong></div>
</div>
<div class="card">
<h2>Business Details</h2>
<label>Business Name</label>
<input id="bizName" value="Chingari Creation">
<label>Mobile Number</label>
<input id="bizPhone" placeholder="Mobile number">
<label>GSTIN (Optional)</label>
<input id="bizGst" placeholder="GST number">
<button class="primary" onclick="saveBusiness()">Save Details</button>
<p class="muted">Data is saved in this browser on this device.</p>
</div>
<div class="card"><h2>Recent Bills</h2><div id="recentBills">No bills yet</div></div>
</section>

<section id="billing" class="hidden">
<div class="card">
<h2>Create Invoice</h2>
<div class="row">
<div><label>Invoice Number</label><input id="invoiceNo" readonly></div>
<div><label>Date</label><input id="billDate" type="date"></div>
<div><label>Customer</label><select id="billCustomer"><option value="">Walk-in Customer</option></select></div>
</div>
<h3>Items</h3>
<div class="row">
<div><label>Product</label><select id="billProduct"><option value="">Select Product</option></select></div>
<div><label>Quantity</label><input id="billQty" type="number" min="1" value="1"></div>
<div><label>Add Item</label><button class="primary" onclick="addItem()">Add Item</button></div>
</div>
<div class="table-wrap">
<table><thead><tr><th>Product</th><th>Price</th><th>Qty</th><th>Amount</th><th>Action</th></tr></thead><tbody id="billItems"></tbody></table>
</div>
<div class="row">
<div><label>Discount ₹</label><input id="billDiscount" type="number" min="0" value="0" oninput="renderItems()"></div>
<div><label>GST %</label><input id="billTax" type="number" min="0" value="0" oninput="renderItems()"></div>
<div><label>Payment</label><select id="billPayment"><option>Cash</option><option>UPI</option><option>Card</option><option>Bank Transfer</option><option>Credit</option></select></div>
</div>
<div id="billTotals" class="total">Total: ₹0</div>
<div class="actions no-print">
<button class="primary" onclick="saveBill()">Save Bill</button>
<button onclick="window.print()">Print</button>
<button onclick="clearBill()">Clear</button>
</div>
</div>
</section>

<section id="products" class="hidden">
<div class="card">
<h2>Products</h2>
<label>Product Name</label><input id="pName" placeholder="Product name">
<label>Price ₹</label><input id="pPrice" type="number" min="0" step="0.01">
<label>Stock Quantity</label><input id="pStock" type="number" min="0" value="0">
<button class="primary" onclick="addProduct()">Add Product</button>
</div>
<div class="card table-wrap"><table><thead><tr><th>Name</th><th>Price</th><th>Stock</th><th>Action</th></tr></thead><tbody id="productRows"></tbody></table></div>
</section>

<section id="customers" class="hidden">
<div class="card">
<h2>Customers</h2>
<label>Customer Name</label><input id="cName">
<label>Mobile</label><input id="cPhone" inputmode="tel">
<label>Address</label><input id="cAddress">
<button class="primary" onclick="addCustomer()">Add Customer</button>
</div>
<div class="card table-wrap"><table><thead><tr><th>Name</th><th>Mobile</th><th>Address</th><th>Action</th></tr></thead><tbody id="customerRows"></tbody></table></div>
</section>

<section id="sales" class="hidden">
<div class="card"><h2>Sales History</h2><div class="table-wrap"><table><thead><tr><th>Invoice</th><th>Date</th><th>Customer</th><th>Payment</th><th>Total</th><th>Action</th></tr></thead><tbody id="salesRows"></tbody></table></div></div>
</section>

<section id="backup" class="hidden">
<div class="card">
<h2>Backup and Restore</h2>
<p>Download your products, customers and bills backup.</p>
<button class="primary" onclick="exportData()">Download Backup</button>
<label>Restore Backup JSON</label><input type="file" id="restoreFile" accept="application/json" onchange="importData(event)">
</div>
</section>
</div>

<script>
const KEY="chingariBillingV1";
function emptyDB(){return {products:[],customers:[],bills:[],business:{name:"Chingari Creation",phone:"",gst:""}}}
function loadDB(){try{return JSON.parse(localStorage.getItem(KEY))||emptyDB()}catch(e){return emptyDB()}}
let db=loadDB(),cart=[];

function money(n){return "₹"+Number(n||0).toLocaleString("en-IN",{minimumFractionDigits:2,maximumFractionDigits:2})}
function esc(s){return String(s??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function saveDB(){localStorage.setItem(KEY,JSON.stringify(db));refresh()}

document.querySelectorAll("[data-tab]").forEach(b=>b.onclick=()=>{
 document.querySelectorAll("[data-tab]").forEach(x=>x.classList.toggle("active",x===b));
 document.querySelectorAll("section").forEach(s=>s.classList.add("hidden"));
 document.getElementById(b.dataset.tab).classList.remove("hidden");
 if(b.dataset.tab==="billing")setInvoice();
});

function saveBusiness(){
 db.business={
 name:document.getElementById("bizName").value.trim()||"Chingari Creation",
 phone:document.getElementById("bizPhone").value.trim(),
 gst:document.getElementById("bizGst").value.trim()
 };
 saveDB();alert("Business details saved");
}

function addProduct(){
 let name=document.getElementById("pName").value.trim();
 let price=Number(document.getElementById("pPrice").value);
 let stock=Number(document.getElementById("pStock").value);
 if(!name||document.getElementById("pPrice").value===""||price<0||stock<0)return alert("Enter valid details");
 db.products.push({id:Date.now(),name,price,stock});
 document.getElementById("pName").value="";
 document.getElementById("pPrice").value="";
 document.getElementById("pStock").value="0";
 saveDB();
}

function delProduct(id){
 if(confirm("Delete this product?")){
 db.products=db.products.filter(p=>p.id!==id);saveDB();
 }
}

function addCustomer(){
 let name=document.getElementById("cName").value.trim();
 if(!name)return alert("Enter customer name");
 db.customers.push({
 id:Date.now(),name,
 phone:document.getElementById("cPhone").value.trim(),
 address:document.getElementById("cAddress").value.trim()
 });
 ["cName","cPhone","cAddress"].forEach(id=>document.getElementById(id).value="");
 saveDB();
}

function delCustomer(id){
 if(confirm("Delete this customer?")){
 db.customers=db.customers.filter(c=>c.id!==id);saveDB();
 }
}

function setInvoice(){
 document.getElementById("invoiceNo").value="CC-"+String(db.bills.length+1).padStart(4,"0");
 document.getElementById("billDate").value=new Date().toISOString().slice(0,10);
 refreshSelects();
}

function refreshSelects(){
 let p=document.getElementById("billProduct"),c=document.getElementById("billCustomer");
 p.innerHTML='<option value="">Select product</option>'+db.products.map(x=>`<option value="${x.id}">${esc(x.name)} — ${money(x.price)} (Stock ${x.stock})</option>`).join("");
 let old=c.value;
 c.innerHTML='<option value="">Walk-in Customer</option>'+db.customers.map(x=>`<option value="${x.id}">${esc(x.name)}</option>`).join("");
 if(db.customers.some(x=>String(x.id)===old))c.value=old;
}

function addItem(){
 let id=Number(document.getElementById("billProduct").value);
 let qty=Math.floor(Number(document.getElementById("billQty").value));
 let p=db.products.find(x=>x.id===id);
 if(!p)return alert("Select a product");
 if(!qty||qty<1)return alert("Enter valid quantity");
 let existing=cart.find(x=>x.id===id);
 if((existing?existing.qty:0)+qty>p.stock)return alert("Available stock: "+p.stock);
 if(existing)existing.qty+=qty;
 else cart.push({id:p.id,name:p.name,price:p.price,qty});
 renderItems();
}

function calc(){
 let sub=cart.reduce((s,x)=>s+x.price*x.qty,0);
 let disc=Math.max(0,Number(document.getElementById("billDiscount").value)||0);
 let tax=Math.max(0,Number(document.getElementById("billTax").value)||0);
 let base=Math.max(0,sub-disc);
 return {sub,disc,taxAmount:base*tax/100,total:base+base*tax/100};
}

function renderItems(){
 document.getElementById("billItems").innerHTML=cart.map((x,i)=>`<tr><td>${esc(x.name)}</td><td>${money(x.price)}</td><td>${x.qty}</td><td>${money(x.price*x.qty)}</td><td><button class="danger" onclick="removeItem(${i})">Remove</button></td></tr>`).join("");
 let t=calc();
 document.getElementById("billTotals").innerHTML=`Subtotal: ${money(t.sub)}<br>Discount: ${money(t.disc)}<br>GST: ${money(t.taxAmount)}<br><b>Grand Total: ${money(t.total)}</b>`;
}

function removeItem(i){cart.splice(i,1);renderItems()}
function clearBill(){
 if(confirm("Clear current bill?")){
 cart=[];document.getElementById("billDiscount").value=0;
 document.getElementById("billTax").value=0;renderItems();
 }
}

function saveBill(){
 if(!cart.length)return alert("Add at least one item");
 let t=calc(),cid=Number(document.getElementById("billCustomer").value);
 let customer=db.customers.find(x=>x.id===cid);
 for(let x of cart){
 let p=db.products.find(p=>p.id===x.id);
 if(!p||p.stock<x.qty)return alert("Please check stock for "+x.name);
 }
 let bill={
 id:Date.now(),invoice:document.getElementById("invoiceNo").value,
 date:document.getElementById("billDate").value,
 customer:customer?customer.name:"Walk-in Customer",
 customerPhone:customer?customer.phone:"",
 payment:document.getElementById("billPayment").value,
 items:cart.map(x=>({...x})),subtotal:t.sub,discount:t.disc,
 taxRate:Number(document.getElementById("billTax").value)||0,
 taxAmount:t.taxAmount,total:t.total,business:{...db.business}
 };
 cart.forEach(x=>{db.products.find(p=>p.id===x.id).stock-=x.qty});
 db.bills.unshift(bill);saveDB();
 alert("Bill saved");
 cart=[];document.getElementById("billDiscount").value=0;
 document.getElementById("billTax").value=0;setInvoice();renderItems();
 printBill(bill);
}

function printBill(b){
 let w=window.open("","_blank");
 if(!w)return alert("Allow pop-ups to print invoice");
 w.document.write(`<html><head><title>${esc(b.invoice)}</title><style>body{font-family:Arial;padding:24px}h1{text-align:center}table{width:100%;border-collapse:collapse;margin-top:20px}td,th{border-bottom:1px solid #ddd;padding:9px;text-align:left}.tot{text-align:right;margin-top:20px;line-height:1.8}</style></head><body><h1>${esc(b.business.name)}</h1><p style="text-align:center">${esc(b.business.phone)} ${b.business.gst?" GSTIN: "+esc(b.business.gst):""}</p><hr><h2>Invoice: ${esc(b.invoice)}</h2><p>Date: ${esc(b.date)}<br>Customer: ${esc(b.customer)}<br>Payment: ${esc(b.payment)}</p><table><tr><th>Item</th><th>Price</th><th>Qty</th><th>Amount</th></tr>${b.items.map(x=>`<tr><td>${esc(x.name)}</td><td>${money(x.price)}</td><td>${x.qty}</td><td>${money(x.price*x.qty)}</td></tr>`).join("")}</table><div class="tot">Subtotal: ${money(b.subtotal)}<br>Discount: ${money(b.discount)}<br>GST (${b.taxRate}%): ${money(b.taxAmount)}<h2>Grand Total: ${money(b.total)}</h2></div><p style="text-align:center;margin-top:50px">Thank you for your business!</p><script>window.onload=()=>window.print()<\/script></body></html>`);
 w.document.close();
}

function showBill(id){let b=db.bills.find(x=>x.id===id);if(b)printBill(b)}

function exportData(){
 let blob=new Blob([JSON.stringify(db,null,2)],{type:"application/json"});
 let a=document.createElement("a");a.href=URL.createObjectURL(blob);
 a.download="chingari-billing-backup.json";a.click();
 URL.revokeObjectURL(a.href);
}

function importData(e){
 let f=e.target.files[0];if(!f)return;
 let r=new FileReader();
 r.onload=()=>{
 try{
 let d=JSON.parse(r.result);
 if(!Array.isArray(d.products)||!Array.isArray(d.customers)||!Array.isArray(d.bills))throw Error();
 if(confirm("Replace current data with backup?")){db=d;saveDB();alert("Backup restored")}
 }catch(err){alert("Invalid backup file")}
 e.target.value="";
 };
 r.readAsText(f);
}

function refresh(){
 document.getElementById("statSales").textContent=money(db.bills.reduce((s,b)=>s+b.total,0));
 document.getElementById("statBills").textContent=db.bills.length;
 document.getElementById("statProducts").textContent=db.products.length;
 document.getElementById("statCustomers").textContent=db.customers.length;
 document.getElementById("bizName").value=db.business.name||"Chingari Creation";
 document.getElementById("bizPhone").value=db.business.phone||"";
 document.getElementById("bizGst").value=db.business.gst||"";
 document.getElementById("productRows").innerHTML=db.products.map(p=>`<tr><td>${esc(p.name)}</td><td>${money(p.price)}</td><td>${p.stock}</td><td><button class="danger" onclick="delProduct(${p.id})">Delete</button></td></tr>`).join("");
 document.getElementById("customerRows").innerHTML=db.customers.map(c=>`<tr><td>${esc(c.name)}</td><td>${esc(c.phone)}</td><td>${esc(c.address)}</td><td><button class="danger" onclick="delCustomer(${c.id})">Delete</button></td></tr>`).join("");
 document.getElementById("salesRows").innerHTML=db.bills.map(b=>`<tr><td>${esc(b.invoice)}</td><td>${esc(b.date)}</td><td>${esc(b.customer)}</td><td>${esc(b.payment)}</td><td>${money(b.total)}</td><td><button onclick="showBill(${b.id})">Print</button></td></tr>`).join("")||'<tr><td colspan="6">No sales yet</td></tr>';
 document.getElementById("recentBills").innerHTML=db.bills.slice(0,5).map(b=>`<p>${esc(b.invoice)} — ${esc(b.customer)} <b style="float:right">${money(b.total)}</b></p>`).join("")||"No bills yet";
 refreshSelects();
}
setInvoice();refresh();renderItems();
</script>
</body>
</html>
