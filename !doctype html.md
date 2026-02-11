<!doctype html>  
<html lang="en" class="h-full">  
 <head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <title>Print Optimizer</title>  
  <script src="https://cdn.tailwindcss.com"></script>  
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>  
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&amp;display=swap" rel="stylesheet">  
  <style>  
    body {  
      box-sizing: border-box;  
      font-family: 'Space Grotesk', sans-serif;  
    }  
    .drop-zone {  
      transition: all 0.3s ease;  
    }  
    .drop-zone.drag-over {  
      border-color: #ec4899;  
      background: rgba(236, 72, 153, 0.1);  
    }  
    .canvas-preview {  
      background-image:   
        linear-gradient(45deg, #f0f0f0 25%, transparent 25%),  
        linear-gradient(-45deg, #f0f0f0 25%, transparent 25%),  
        linear-gradient(45deg, transparent 75%, #f0f0f0 75%),  
        linear-gradient(-45deg, transparent 75%, #f0f0f0 75%);  
      background-size: 20px 20px;  
      background-position: 0 0, 0 10px, 10px -10px, -10px 0px;  
    }  
    @keyframes pulse-border {  
      0%, 100% { border-color: rgba(236, 72, 153, 0.5); }  
      50% { border-color: rgba(236, 72, 153, 1); }  
    }  
    .optimizing {  
      animation: pulse-border 1s ease-in-out infinite;  
    }  
    .margin-guide {  
      position: absolute;  
      border: 1px dashed #f43f5e;  
      pointer-events: none;  
    }  
  </style>  
  <style>@view-transition { navigation: auto; }</style>  
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>  
  <script src="/_sdk/element_sdk.js" type="text/javascript"></script>  
 </head>  
 <body class="h-full bg-slate-900 text-white overflow-auto">  
  <div class="min-h-full p-6"><!-- Header -->  
   <header class="mb-8">  
    <h1 class="text-3xl font-bold bg-gradient-to-r from-pink-400 to-rose-400 bg-clip-text text-transparent">Print Optimizer</h1>  
    <p class="text-slate-400 mt-2">Maximize paper yield with intelligent nesting algorithms</p>  
    <p class="text-slate-500 text-sm mt-3">Designed by Scott Langley</p>  
   </header>  
   <div class="grid grid-cols-1 lg:grid-cols-3 gap-6"><!-- Left Panel: Settings & Upload -->  
    <div class="space-y-6"><!-- Paper Settings -->  
     <div class="bg-slate-800 rounded-2xl p-6 border border-slate-700">  
      <h2 class="text-lg font-semibold text-pink-300 mb-4 flex items-center gap-2">  
       <svg class="w-5 h-5" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />  
       </svg> Paper Sheet</h2>  
      <div class="space-y-4">  
       <div><label class="block text-sm text-slate-400 mb-2">Width</label>  
        <div class="flex gap-2"><input type="number" id="paper-width" value="50" min="1" step="0.1" class="flex-1 bg-slate-700 border border-slate-600 rounded-lg px-4 py-2.5 text-white focus:border-pink-500 focus:outline-none"> <select id="width-unit" class="bg-slate-700 border border-slate-600 rounded-lg px-4 py-2.5 text-white focus:border-pink-500 focus:outline-none whitespace-nowrap"> <option value="in">inches</option> <option value="cm">cm</option> <option value="mm">mm</option> </select>  
        </div>  
       </div>  
       <div><label class="block text-sm text-slate-400 mb-2">Margin</label>  
        <div class="flex gap-2"><input type="number" id="margin" value="0.25" min="0" step="0.125" class="flex-1 bg-slate-700 border border-slate-600 rounded-lg px-4 py-2.5 text-white focus:border-pink-500 focus:outline-none"> <span class="bg-slate-700 border border-slate-600 rounded-lg px-4 py-2.5 text-slate-400 whitespace-nowrap">in</span>  
        </div>  
       </div>  
      </div>  
     </div><!-- Upload Zone -->  
     <div class="bg-slate-800 rounded-2xl p-6 border border-slate-700">  
      <h2 class="text-lg font-semibold text-pink-300 mb-4 flex items-center gap-2">  
       <svg class="w-5 h-5" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />  
       </svg> Upload Art Files</h2>  
      <div id="drop-zone" class="drop-zone border-2 border-dashed border-slate-600 rounded-xl p-8 text-center cursor-pointer hover:border-pink-500 hover:bg-slate-700/50"><input type="file" id="file-input" multiple accept=".png,.jpg,.jpeg,.gif,.svg,.pdf" class="hidden">  
       <svg class="w-12 h-12 mx-auto text-slate-500 mb-3" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m0 0h6m-6-6h-6m0 0H6" />  
       </svg>  
       <p class="text-slate-400 text-sm">Drop art files here</p>  
       <p class="text-slate-500 text-xs mt-1">or click to browse</p>  
      </div>  
     </div>  
    </div><!-- Center: Layout Preview -->  
    <div class="lg:col-span-2 space-y-6"><!-- Action Bar -->  
     <div class="flex flex-wrap gap-3"><button id="optimize-btn" class="flex-1 bg-gradient-to-r from-pink-600 to-rose-600 hover:from-pink-500 hover:to-rose-500 text-white font-semibold py-3 px-6 rounded-xl transition-all flex items-center justify-center gap-2">  
       <svg class="w-5 h-5" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />  
       </svg> Optimize Layout </button> <button id="save-btn" disabled class="flex-1 bg-slate-700 hover:bg-slate-600 disabled:opacity-50 disabled:cursor-not-allowed text-white font-semibold py-3 px-6 rounded-xl transition-all flex items-center justify-center gap-2">  
       <svg class="w-5 h-5" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v10a2 2 0 01-2 2h-2m-4-6h4m-4 4h4m-5-10h2m-2 0h2" />  
       </svg> Save PDF </button>  
     </div><!-- Stats -->  
     <div class="grid grid-cols-3 gap-4">  
      <div class="bg-slate-800 rounded-xl p-4 border border-slate-700 text-center">  
       <p class="text-2xl font-bold text-pink-400" id="paper-used">0%</p>  
       <p class="text-xs text-slate-400 mt-1">Paper Utilization</p>  
      </div>  
      <div class="bg-slate-800 rounded-xl p-4 border border-slate-700 text-center">  
       <p class="text-2xl font-bold text-emerald-400" id="designs-nested">0</p>  
       <p class="text-xs text-slate-400 mt-1">Art Files Nested</p>  
      </div>  
      <div class="bg-slate-800 rounded-xl p-4 border border-slate-700 text-center">  
       <p class="text-2xl font-bold text-amber-400" id="waste-area">0 sq in</p>  
       <p class="text-xs text-slate-400 mt-1">Waste Material</p>  
      </div>  
     </div><!-- Canvas Preview -->  
     <div class="bg-slate-800 rounded-2xl p-6 border border-slate-700">  
      <div class="flex items-center justify-between mb-4">  
       <h2 class="text-lg font-semibold text-pink-300">Print Layout</h2>  
       <div class="flex items-center gap-2"><button id="zoom-out" class="p-2 bg-slate-700 hover:bg-slate-600 rounded-lg transition-colors">  
         <svg class="w-4 h-4" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 12H4" />  
         </svg></button> <span id="zoom-level" class="text-sm text-slate-400 w-12 text-center">100%</span> <button id="zoom-in" class="p-2 bg-slate-700 hover:bg-slate-600 rounded-lg transition-colors">  
         <svg class="w-4 h-4" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />  
         </svg></button>  
       </div>  
      </div>  
      <div id="canvas-container" class="canvas-preview rounded-xl overflow-auto border border-slate-600" style="height: 500px;">  
       <div id="layout-canvas" class="relative bg-white" style="min-height: 100%;">  
        <p class="absolute inset-0 flex items-center justify-center text-slate-400">Add art files and click Optimize to see layout</p>  
       </div>  
      </div>  
     </div>  
    </div>  
   </div>  
  </div>  
  <script>  
    let designs = [];  
    let nestedDesigns = [];  
    let zoomLevel = 100;  
    let paperWidthInches = 50;  
    let paperHeightInches = 11;  
    let marginInches = 0.25;  
  
    const dropZone = document.getElementById('drop-zone');  
    const fileInput = document.getElementById('file-input');  
  
    dropZone.addEventListener('click', () => fileInput.click());  
    dropZone.addEventListener('dragover', (e) => {  
      e.preventDefault();  
      dropZone.classList.add('drag-over');  
    });  
    dropZone.addEventListener('dragleave', () => {  
      dropZone.classList.remove('drag-over');  
    });  
    dropZone.addEventListener('drop', (e) => {  
      e.preventDefault();  
      dropZone.classList.remove('drag-over');  
      handleFiles(e.dataTransfer.files);  
    });  
    fileInput.addEventListener('change', (e) => {  
      handleFiles(e.target.files);  
    });  
  
    async function handleFiles(files) {  
      for (const file of files) {  
        const id = Date.now() + Math.random().toString(36).substr(2, 9);  
        let width = 3;  
        let height = 4;  
          
        if (file.type.startsWith('image/')) {  
          const dims = await getImageDimensions(file);  
          width = dims.width;  
          height = dims.height;  
        }  
          
        designs.push({  
          id,  
          name: file.name.split('.')[0],  
          width: Math.round(width * 100) / 100,  
          height: Math.round(height * 100) / 100,  
          quantity: 1,  
          color: getRandomColor()  
        });  
      }  
    }  
  
    function getImageDimensions(file) {  
      return new Promise((resolve) => {  
        const img = new Image();  
        const url = URL.createObjectURL(file);  
        img.onload = () => {  
          const width = img.naturalWidth / 96;  
          const height = img.naturalHeight / 96;  
          URL.revokeObjectURL(url);  
          resolve({ width, height });  
        };  
        img.onerror = () => {  
          URL.revokeObjectURL(url);  
          resolve({ width: 3, height: 4 });  
        };  
        img.src = url;  
      });  
    }  
  
    function getRandomColor() {  
      const colors = ['#ec4899', '#f43f5e', '#f97316', '#eab308', '#22c55e', '#14b8a6', '#0ea5e9', '#3b82f6', '#8b5cf6', '#d946ef'];  
      return colors[Math.floor(Math.random() * colors.length)];  
    }  
  
    document.getElementById('paper-width').addEventListener('change', (e) => {  
      const unit = document.getElementById('width-unit').value;  
      let value = parseFloat(e.target.value) || 50;  
      if (unit === 'cm') value /= 2.54;  
      else if (unit === 'mm') value /= 25.4;  
      paperWidthInches = value;  
    });  
  
    document.getElementById('width-unit').addEventListener('change', () => {  
      const value = parseFloat(document.getElementById('paper-width').value) || 50;  
      const unit = document.getElementById('width-unit').value;  
      if (unit === 'cm') paperWidthInches = value / 2.54;  
      else if (unit === 'mm') paperWidthInches = value / 25.4;  
      else paperWidthInches = value;  
    });  
  
    document.getElementById('margin').addEventListener('change', (e) => {  
      marginInches = parseFloat(e.target.value) || 0.25;  
    });  
  
    function optimizeLayout() {  
      const canvas = document.getElementById('layout-canvas');  
      const optimizeBtn = document.getElementById('optimize-btn');  
        
      if (designs.length === 0) {  
        canvas.innerHTML = '<p class="absolute inset-0 flex items-center justify-center text-slate-400">Add art files first</p>';  
        return;  
      }  
        
      optimizeBtn.disabled = true;  
      canvas.classList.add('optimizing');  
        
      let items = [];  
      designs.forEach(design => {  
        for (let i = 0; i < design.quantity; i++) {  
          items.push({  
            ...design,  
            instanceId: `${design.id}-${i}`,  
            rotated: false  
          });  
        }  
      });  
        
      items.sort((a, b) => (b.width * b.height) - (a.width * a.height));  
        
      const usableWidth = paperWidthInches - (marginInches * 2);  
      const usableHeight = paperHeightInches - (marginInches * 2);  
        
      let rows = [];  
      let currentY = marginInches;  
        
      items.forEach(item => {  
        let placed = false;  
        let itemWidth = item.width;  
        let itemHeight = item.height;  
          
        let bestConfig = null;  
          
        if (itemWidth <= usableWidth && itemHeight <= usableHeight) {  
          for (let row of rows) {  
            if (itemWidth <= usableWidth - row.currentX && itemHeight <= row.height) {  
              bestConfig = { row, width: item.width, height: item.height, rotated: false };  
              break;  
            }  
          }  
        }  
          
        let rotatedWidth = item.height;  
        let rotatedHeight = item.width;  
        if (rotatedWidth <= usableWidth && rotatedHeight <= usableHeight && !bestConfig) {  
          for (let row of rows) {  
            if (rotatedWidth <= usableWidth - row.currentX && rotatedHeight <= row.height) {  
              bestConfig = { row, width: item.height, height: item.width, rotated: true };  
              break;  
            }  
          }  
        }  
          
        if (bestConfig) {  
          bestConfig.row.items.push({  
            ...item,  
            x: bestConfig.row.currentX + marginInches,  
            y: bestConfig.row.y,  
            placedWidth: bestConfig.width,  
            placedHeight: bestConfig.height,  
            rotated: bestConfig.rotated  
          });  
          bestConfig.row.currentX += (bestConfig.rotated ? rotatedWidth : itemWidth);  
          placed = true;  
        }  
          
        if (!placed) {  
          let useRotated = false;  
          let newRowHeight = itemHeight;  
          let newRowWidth = itemWidth;  
            
          if (itemWidth > usableWidth && rotatedWidth <= usableWidth) {  
            useRotated = true;  
            newRowHeight = rotatedHeight;  
            newRowWidth = rotatedWidth;  
          } else if (rotatedWidth <= usableWidth && rotatedHeight < itemHeight) {  
            useRotated = true;  
            newRowHeight = rotatedHeight;  
            newRowWidth = rotatedWidth;  
          }  
            
          if (currentY + newRowHeight <= paperHeightInches) {  
            const newRow = {  
              y: currentY,  
              height: newRowHeight,  
              currentX: marginInches + newRowWidth,  
              items: [{  
                ...item,  
                x: marginInches,  
                y: currentY,  
                placedWidth: useRotated ? item.height : item.width,  
                placedHeight: useRotated ? item.width : item.height,  
                rotated: useRotated  
              }]  
            };  
              
            rows.push(newRow);  
            currentY += newRowHeight;  
          }  
        }  
      });  
        
      nestedDesigns = rows.flatMap(r => r.items);  
        
      const paperArea = paperWidthInches * paperHeightInches;  
      const usedArea = nestedDesigns.reduce((sum, d) => sum + (d.placedWidth * d.placedHeight), 0);  
      const utilization = Math.round((usedArea / paperArea) * 100);  
      const wasteArea = paperArea - usedArea;  
        
      document.getElementById('paper-used').textContent = `${utilization}%`;  
      document.getElementById('designs-nested').textContent = `${nestedDesigns.length}`;  
      document.getElementById('waste-area').textContent = `${wasteArea.toFixed(1)} sq in`;  
        
      renderPreview();  
        
      optimizeBtn.disabled = false;  
      canvas.classList.remove('optimizing');  
      document.getElementById('save-btn').disabled = false;  
    }  
  
    function renderPreview() {  
      const canvas = document.getElementById('layout-canvas');  
      const scale = zoomLevel / 100 * 60;  
        
      const canvasWidth = paperWidthInches * scale;  
      const canvasHeight = paperHeightInches * scale;  
        
      canvas.style.width = `${canvasWidth}px`;  
      canvas.style.height = `${canvasHeight}px`;  
        
      let html = `  
        <div class="absolute inset-0 border-3 border-slate-900" style="width: ${canvasWidth}px; height: ${canvasHeight}px;">  
          <div class="absolute top-2 left-2 text-xs text-slate-700 bg-white px-2 py-1 rounded font-semibold">${paperWidthInches}" × ${paperHeightInches}"</div>  
        </div>  
      `;  
        
      const marginPx = marginInches * scale;  
      html += `  
        <div class="margin-guide" style="left: ${marginPx}px; top: ${marginPx}px; width: ${canvasWidth - marginPx * 2}px; height: ${canvasHeight - marginPx * 2}px;"></div>  
      `;  
        
      nestedDesigns.forEach((design) => {  
        const x = design.x * scale;  
        const y = design.y * scale;  
        const w = design.placedWidth * scale;  
        const h = design.placedHeight * scale;  
          
        html += `  
          <div class="absolute border-2 rounded shadow-sm flex items-center justify-center overflow-hidden transition-all hover:shadow-lg hover:z-10"   
               style="left: ${x}px; top: ${y}px; width: ${w}px; height: ${h}px; background: ${design.color}30; border-color: ${design.color}; cursor: pointer;">  
            <div class="text-center pointer-events-none">  
              <div class="text-xs font-bold px-1" style="font-size: ${Math.max(8, Math.min(11, w / 12))}px; color: ${design.color};">  
                ${design.rotated ? '↻ ' : ''}${design.name}  
              </div>  
              <div class="text-xs text-slate-600" style="font-size: ${Math.max(7, Math.min(9, w / 15))}px;">  
                ${design.placedWidth.toFixed(1)}" × ${design.placedHeight.toFixed(1)}"  
              </div>  
            </div>  
          </div>  
        `;  
      });  
        
      canvas.innerHTML = html;  
    }  
  
    document.getElementById('zoom-in').addEventListener('click', () => {  
      zoomLevel = Math.min(200, zoomLevel + 25);  
      document.getElementById('zoom-level').textContent = `${zoomLevel}%`;  
      if (nestedDesigns.length > 0) renderPreview();  
    });  
  
    document.getElementById('zoom-out').addEventListener('click', () => {  
      zoomLevel = Math.max(25, zoomLevel - 25);  
      document.getElementById('zoom-level').textContent = `${zoomLevel}%`;  
      if (nestedDesigns.length > 0) renderPreview();  
    });  
  
    document.getElementById('optimize-btn').addEventListener('click', optimizeLayout);  
  
    document.getElementById('save-btn').addEventListener('click', () => {  
      if (nestedDesigns.length === 0) return;  
      showSaveDialog();  
    });  
  
    function showSaveDialog() {  
      const modalOverlay = document.createElement('div');  
      modalOverlay.className = 'fixed inset-0 bg-black/50 flex items-center justify-center z-50';  
        
      const modal = document.createElement('div');  
      modal.className = 'bg-slate-800 rounded-2xl p-8 border border-slate-700 max-w-md w-full mx-4 shadow-2xl';  
        
      const utilization = document.getElementById('paper-used').textContent;  
      const stats = `${paperWidthInches}" × ${paperHeightInches}" sheet | ${nestedDesigns.length} art files | ${utilization} utilized`;  
        
      modal.innerHTML = `  
        <h3 class="text-2xl font-bold text-white mb-2">Save Print-Optimized PDF</h3>  
        <p class="text-slate-400 text-sm mb-6">All art files will be combined into one optimized layout</p>  
          
        <div class="bg-slate-700/50 rounded-lg p-4 mb-6 border border-slate-600">  
          <p class="text-xs text-slate-400 mb-2">Layout Details</p>  
          <p class="text-sm font-semibold text-pink-300">${stats}</p>  
        </div>  
          
        <div class="mb-6">  
          <label class="block text-sm text-slate-300 font-medium mb-2">Filename</label>  
          <input type="text" id="filename-input" value="print-layout-optimized"   
            class="w-full bg-slate-700 border border-slate-600 rounded-lg px-4 py-3 text-white focus:border-pink-500 focus:outline-none">  
          <p class="text-xs text-slate-500 mt-2">.pdf will be added automatically</p>  
        </div>  
          
        <div class="flex gap-3">  
          <button id="cancel-save" class="flex-1 bg-slate-700 hover:bg-slate-600 text-white font-semibold py-2.5 px-4 rounded-lg transition-colors">  
            Cancel  
          </button>  
          <button id="confirm-save" class="flex-1 bg-gradient-to-r from-pink-600 to-rose-600 hover:from-pink-500 hover:to-rose-500 text-white font-semibold py-2.5 px-4 rounded-lg transition-all flex items-center justify-center gap-2">  
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">  
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v10a2 2 0 01-2 2h-2m-4-6h4m-4 4h4m-5-10h2m-2 0h2"/>  
            </svg>  
            Save PDF  
          </button>  
        </div>  
      `;  
        
      modalOverlay.appendChild(modal);  
      document.body.appendChild(modalOverlay);  
        
      const filenameInput = document.getElementById('filename-input');  
      const cancelBtn = document.getElementById('cancel-save');  
      const confirmBtn = document.getElementById('confirm-save');  
        
      setTimeout(() => filenameInput.focus(), 0);  
      filenameInput.addEventListener('focus', (e) => e.target.select());  
        
      cancelBtn.addEventListener('click', () => modalOverlay.remove());  
        
      confirmBtn.addEventListener('click', async () => {  
        let filename = filenameInput.value.trim();  
        if (!filename) filename = 'print-layout-optimized';  
        if (!filename.endsWith('.pdf')) filename += '.pdf';  
          
        confirmBtn.disabled = true;  
        confirmBtn.innerHTML = '<svg class="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg> Generating...';  
          
        try {  
          await generateAndSavePDF(filename);  
          modalOverlay.remove();  
        } catch (err) {  
          console.error('Save failed:', err);  
          confirmBtn.disabled = false;  
          confirmBtn.innerHTML = '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v10a2 2 0 01-2 2h-2m-4-6h4m-4 4h4m-5-10h2m-2 0h2"/></svg> Save PDF';  
        }  
      });  
        
      filenameInput.addEventListener('keypress', (e) => {  
        if (e.key === 'Enter') confirmBtn.click();  
      });  
        
      document.addEventListener('keydown', function handleEscape(e) {  
        if (e.key === 'Escape') {  
          modalOverlay.remove();  
          document.removeEventListener('keydown', handleEscape);  
        }  
      });  
    }  
  
    async function generateAndSavePDF(filename) {  
      const pdfDoc = await PDFLib.PDFDocument.create();  
      const pageWidth = paperWidthInches * 72;  
      const pageHeight = paperHeightInches * 72;  
      const page = pdfDoc.addPage([pageWidth, pageHeight]);  
        
      page.drawRectangle({  
        x: 0,  
        y: 0,  
        width: pageWidth,  
        height: pageHeight,  
        borderColor: PDFLib.rgb(0, 0, 0),  
        borderWidth: 2  
      });  
        
      const marginPts = marginInches * 72;  
      page.drawRectangle({  
        x: marginPts,  
        y: marginPts,  
        width: pageWidth - marginPts * 2,  
        height: pageHeight - marginPts * 2,  
        borderColor: PDFLib.rgb(0.8, 0.8, 0.8),  
        borderWidth: 1  
      });  
        
      nestedDesigns.forEach((design) => {  
        const x = design.x * 72;  
        const y = pageHeight - (design.y + design.placedHeight) * 72;  
        const w = design.placedWidth * 72;  
        const h = design.placedHeight * 72;  
          
        page.drawRectangle({  
          x,  
          y,  
          width: w,  
          height: h,  
          borderColor: PDFLib.rgb(0.2, 0.2, 0.2),  
          borderWidth: 1  
        });  
          
        page.drawText(`${design.name}${design.rotated ? ' ↻' : ''}`, {  
          x: x + 3,  
          y: y + h - 12,  
          size: 8,  
          color: PDFLib.rgb(0.2, 0.2, 0.2)  
        });  
          
        page.drawText(`${design.placedWidth.toFixed(2)}" × ${design.placedHeight.toFixed(2)}"`, {  
          x: x + 3,  
          y: y + 3,  
          size: 7,  
          color: PDFLib.rgb(0.5, 0.5, 0.5)  
        });  
      });  
        
      const utilization = document.getElementById('paper-used').textContent;  
      page.drawText(`Print Optimizer Layout: ${paperWidthInches}" × ${paperHeightInches}" | ${nestedDesigns.length} art files | ${utilization} paper used`, {  
        x: 10,  
        y: 10,  
        size: 9,  
        color: PDFLib.rgb(0.5, 0.5, 0.5)  
      });  
        
      const pdfBytes = await pdfDoc.save();  
      const blob = new Blob([pdfBytes], { type: 'application/pdf' });  
        
      // Try the file picker first (this should work when running locally)  
      if (window.showSaveFilePicker) {  
        try {  
          const handle = await window.showSaveFilePicker({  
            suggestedName: filename,  
            types: [{  
              description: 'PDF Files',  
              accept: { 'application/pdf': ['.pdf'] }  
            }]  
          });  
          const writable = await handle.createWritable();  
          await writable.write(blob);  
          await writable.close();  
          return;  
        } catch (err) {  
          if (err.name === 'AbortError') {  
            return; // User cancelled  
          }  
          // Fall through to standard download if picker fails  
        }  
      }  
        
      // Fallback: standard download  
      const url = URL.createObjectURL(blob);  
      const link = document.createElement('a');  
      link.href = url;  
      link.download = filename;  
      document.body.appendChild(link);  
      link.click();  
      document.body.removeChild(link);  
      setTimeout(() => URL.revokeObjectURL(url), 100);  
    }  
  </script>  
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9cc6082766144df6',t:'MTc3MDgzNTgyNS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>  
</html>  
