<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eliminasi Gauss-Jordan & Gauss-Seidel</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: Arial, sans-serif; background: #f0f2f5; padding: 20px; }
        .container { max-width: 1200px; margin: 0 auto; background: white; border-radius: 10px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .header { background: #4facfe; color: white; padding: 20px; text-align: center; border-radius: 10px 10px 0 0; }
        .content { padding: 20px; }
        .method-section { margin-bottom: 30px; border: 1px solid #ddd; border-radius: 8px; padding: 20px; }
        .method-title { font-size: 1.5em; color: #4facfe; margin-bottom: 15px; text-align: center; }
        .matrix-input { display: grid; grid-template-columns: repeat(4, 70px); gap: 5px; margin: 10px 0; justify-content: center; }
        .matrix-input input { padding: 8px; border: 1px solid #ddd; border-radius: 4px; text-align: center; }
        .btn { background: #4facfe; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; margin: 5px; }
        .btn:hover { background: #3a8bfe; }
        .results { margin-top: 20px; padding: 15px; background: #f8f9fa; border-radius: 5px; }
        .step { margin: 15px 0; padding: 15px; background: white; border-radius: 5px; border-left: 3px solid #4facfe; }
        .matrix-display { font-family: monospace; background: white; padding: 15px; border: 1px solid #ddd; border-radius: 5px; margin: 10px 0; }
        .matrix-row { display: flex; justify-content: center; margin: 5px 0; }
        .matrix-element { padding: 8px 10px; margin: 2px; background: #f8f9fa; border: 1px solid #ddd; border-radius: 3px; min-width: 60px; text-align: center; }
        .matrix-element.pivot { background: #ffeb3b; border-color: #ff9800; }
        .solution { background: #e8f5e8; border: 2px solid #4caf50; padding: 20px; border-radius: 8px; margin: 15px 0; text-align: center; }
        .variable-result { font-size: 1.2em; margin: 10px 0; padding: 10px; background: white; border-radius: 5px; font-weight: bold; }
        .error { background: #ffebee; border: 2px solid #f44336; padding: 15px; border-radius: 5px; color: #c62828; margin: 10px 0; }
        .iteration-table { width: 100%; border-collapse: collapse; margin: 15px 0; }
        .iteration-table th, .iteration-table td { border: 1px solid #ddd; padding: 8px; text-align: center; }
        .iteration-table th { background: #4facfe; color: white; }
        .tolerance-section { margin: 15px 0; display: flex; gap: 10px; align-items: center; justify-content: center; flex-wrap: wrap; }
        .tolerance-section input { padding: 5px; border: 1px solid #ddd; border-radius: 3px; width: 80px; text-align: center; }
        @media (max-width: 768px) { .matrix-input { grid-template-columns: repeat(2, 70px); } }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Eliminasi Gauss-Jordan & Gauss-Seidel</h1>
            <p>Solusi Sistem Persamaan Linear</p>
        </div>
        
        <div class="content">
            <div class="method-section">
                <h2 class="method-title">Metode Eliminasi Gauss-Jordan</h2>
                <h3>Input SPL 3×3</h3>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₁₁" id="gj_0_0">
                    <input type="number" step="any" placeholder="a₁₂" id="gj_0_1">
                    <input type="number" step="any" placeholder="a₁₃" id="gj_0_2">
                    <input type="number" step="any" placeholder="b₁" id="gj_0_3">
                </div>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₂₁" id="gj_1_0">
                    <input type="number" step="any" placeholder="a₂₂" id="gj_1_1">
                    <input type="number" step="any" placeholder="a₂₃" id="gj_1_2">
                    <input type="number" step="any" placeholder="b₂" id="gj_1_3">
                </div>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₃₁" id="gj_2_0">
                    <input type="number" step="any" placeholder="a₃₂" id="gj_2_1">
                    <input type="number" step="any" placeholder="a₃₃" id="gj_2_2">
                    <input type="number" step="any" placeholder="b₃" id="gj_2_3">
                </div>
                <button class="btn" onclick="solveGaussJordan()">Selesaikan Gauss-Jordan</button>
                <button class="btn" onclick="clearGJ()">Bersihkan</button>
                <div id="gaussJordanResults" class="results" style="display: none;"></div>
            </div>

            <div class="method-section">
                <h2 class="method-title">Metode Gauss-Seidel</h2>
                <h3>Input SPL 3×3</h3>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₁₁" id="gs_0_0">
                    <input type="number" step="any" placeholder="a₁₂" id="gs_0_1">
                    <input type="number" step="any" placeholder="a₁₃" id="gs_0_2">
                    <input type="number" step="any" placeholder="b₁" id="gs_0_3">
                </div>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₂₁" id="gs_1_0">
                    <input type="number" step="any" placeholder="a₂₂" id="gs_1_1">
                    <input type="number" step="any" placeholder="a₂₃" id="gs_1_2">
                    <input type="number" step="any" placeholder="b₂" id="gs_1_3">
                </div>
                <div class="matrix-input">
                    <input type="number" step="any" placeholder="a₃₁" id="gs_2_0">
                    <input type="number" step="any" placeholder="a₃₂" id="gs_2_1">
                    <input type="number" step="any" placeholder="a₃₃" id="gs_2_2">
                    <input type="number" step="any" placeholder="b₃" id="gs_2_3">
                </div>
                <div class="tolerance-section">
                    <label>Toleransi:</label>
                    <input type="number" step="any" value="0.0001" id="tolerance">
                    <label>Max Iterasi:</label>
                    <input type="number" value="100" id="maxIterations">
                </div>
                <button class="btn" onclick="solveGaussSeidel()">Selesaikan Gauss-Seidel</button>
                <button class="btn" onclick="clearGS()">Bersihkan</button>
                <div id="gaussSeidelResults" class="results" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        function clearGJ() {
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 4; j++) {
                    document.getElementById(`gj_${i}_${j}`).value = '';
                }
            }
            document.getElementById('gaussJordanResults').style.display = 'none';
        }

        function clearGS() {
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 4; j++) {
                    document.getElementById(`gs_${i}_${j}`).value = '';
                }
            }
            document.getElementById('gaussSeidelResults').style.display = 'none';
        }

        function displayMatrix(matrix, title = "", pivotRow = -1, pivotCol = -1) {
            let html = `<div class="matrix-display">`;
            if (title) html += `<h4>${title}</h4>`;
            for (let i = 0; i < matrix.length; i++) {
                html += `<div class="matrix-row">`;
                for (let j = 0; j < matrix[i].length; j++) {
                    const value = typeof matrix[i][j] === 'number' ? 
                        (Math.abs(matrix[i][j]) < 1e-10 ? '0' : matrix[i][j].toFixed(4)) : matrix[i][j];
                    const isPivot = (i === pivotRow && j === pivotCol);
                    const className = isPivot ? 'matrix-element pivot' : 'matrix-element';
                    html += `<div class="${className}">${value}</div>`;
                }
                html += `</div>`;
            }
            html += `</div>`;
            return html;
        }

        function solveGaussJordan() {
            try {
                const matrix = [];
                for (let i = 0; i < 3; i++) {
                    matrix[i] = [];
                    for (let j = 0; j < 4; j++) {
                        const value = parseFloat(document.getElementById(`gj_${i}_${j}`).value);
                        if (isNaN(value)) throw new Error(`Nilai pada baris ${i+1}, kolom ${j+1} tidak valid`);
                        matrix[i][j] = value;
                    }
                }

                let html = `<h3>Langkah-langkah Eliminasi Gauss-Jordan</h3>`;
                html += displayMatrix(matrix, "Matriks Awal:");

                let stepCount = 1;
                for (let i = 0; i < 3; i++) {
                    // Partial pivoting
                    let maxRow = i;
                    for (let k = i + 1; k < 3; k++) {
                        if (Math.abs(matrix[k][i]) > Math.abs(matrix[maxRow][i])) maxRow = k;
                    }
                    if (maxRow !== i) {
                        [matrix[i], matrix[maxRow]] = [matrix[maxRow], matrix[i]];
                        html += `<div class="step"><h4>Langkah ${stepCount++}: Tukar baris ${i+1} dengan ${maxRow+1}</h4>`;
                        html += displayMatrix(matrix, "", i, i);
                        html += `</div>`;
                    }

                    if (Math.abs(matrix[i][i]) < 1e-10) throw new Error(`Sistem tidak memiliki solusi unik`);

                    // Normalisasi
                    const pivot = matrix[i][i];
                    if (Math.abs(pivot - 1) > 1e-10) {
                        for (let j = 0; j < 4; j++) matrix[i][j] /= pivot;
                        html += `<div class="step"><h4>Langkah ${stepCount++}: R${i+1} = R${i+1} ÷ ${pivot.toFixed(4)}</h4>`;
                        html += displayMatrix(matrix, "", i, i);
                        html += `</div>`;
                    }

                    // Eliminasi
                    for (let k = 0; k < 3; k++) {
                        if (k !== i && Math.abs(matrix[k][i]) > 1e-10) {
                            const factor = matrix[k][i];
                            for (let j = 0; j < 4; j++) matrix[k][j] -= factor * matrix[i][j];
                            html += `<div class="step"><h4>Langkah ${stepCount++}: R${k+1} = R${k+1} - ${factor.toFixed(4)} × R${i+1}</h4>`;
                            html += displayMatrix(matrix, "", i, i);
                            html += `</div>`;
                        }
                    }
                }

                html += `<div class="solution"><h3>Solusi</h3>`;
                html += `<div class="variable-result">x₁ = ${matrix[0][3].toFixed(6)}</div>`;
                html += `<div class="variable-result">x₂ = ${matrix[1][3].toFixed(6)}</div>`;
                html += `<div class="variable-result">x₃ = ${matrix[2][3].toFixed(6)}</div></div>`;

                // Add verification section after the solution
                html += `<div class="step">`;
                html += `<h4>🔍 Pembuktian/Verifikasi Solusi</h4>`;
                html += `<p>Substitusi nilai x₁, x₂, x₃ ke dalam persamaan asli:</p>`;

                // Store original matrix values for verification
                const originalMatrix = [];
                for (let i = 0; i < 3; i++) {
                    originalMatrix[i] = [];
                    for (let j = 0; j < 4; j++) {
                        originalMatrix[i][j] = parseFloat(document.getElementById(`gj_${i}_${j}`).value);
                    }
                }

                // Verify each equation
                let allCorrect = true;
                for (let i = 0; i < 3; i++) {
                    const x1 = matrix[0][3];
                    const x2 = matrix[1][3];
                    const x3 = matrix[2][3];
                    
                    // Calculate left side of equation
                    const leftSide = originalMatrix[i][0] * x1 + originalMatrix[i][1] * x2 + originalMatrix[i][2] * x3;
                    const rightSide = originalMatrix[i][3];
                    const difference = Math.abs(leftSide - rightSide);
                    
                    // Build equation string
                    let equationStr = `Persamaan ${i+1}: `;
                    equationStr += `(${originalMatrix[i][0]}) × (${x1.toFixed(6)}) + `;
                    equationStr += `(${originalMatrix[i][1]}) × (${x2.toFixed(6)}) + `;
                    equationStr += `(${originalMatrix[i][2]}) × (${x3.toFixed(6)}) = `;
                    equationStr += `${leftSide.toFixed(6)}`;
                    
                    const isCorrect = difference < 1e-10;
                    if (!isCorrect) allCorrect = false;
                    
                    html += `<div style="margin: 10px 0; padding: 10px; background: ${isCorrect ? '#e8f5e8' : '#ffebee'}; border-radius: 5px; border-left: 3px solid ${isCorrect ? '#4caf50' : '#f44336'};">`;
                    html += `<div style="font-family: monospace; font-size: 0.9em; margin-bottom: 5px;">${equationStr}</div>`;
                    html += `<div style="font-weight: bold;">Seharusnya = ${rightSide}</div>`;
                    html += `<div style="color: ${isCorrect ? '#2e7d32' : '#c62828'}; font-weight: bold;">`;
                    html += `${isCorrect ? '✅ BENAR' : '❌ SALAH'} (Selisih: ${difference.toFixed(10)})`;
                    html += `</div></div>`;
                }

                html += `<div style="margin-top: 15px; padding: 15px; background: ${allCorrect ? '#e8f5e8' : '#ffebee'}; border: 2px solid ${allCorrect ? '#4caf50' : '#f44336'}; border-radius: 8px; text-align: center;">`;
                html += `<h4 style="color: ${allCorrect ? '#2e7d32' : '#c62828'}; margin-bottom: 10px;">`;
                html += `${allCorrect ? '🎉 PEMBUKTIAN BERHASIL!' : '⚠️ PEMBUKTIAN GAGAL!'}`;
                html += `</h4>`;
                html += `<p style="font-weight: bold;">`;
                html += `${allCorrect ? 'Semua persamaan terpenuhi dengan benar. Solusi valid!' : 'Ada kesalahan dalam perhitungan. Periksa kembali input atau metode.'}`;
                html += `</p></div>`;
                html += `</div>`;

                document.getElementById('gaussJordanResults').innerHTML = html;
                document.getElementById('gaussJordanResults').style.display = 'block';
            } catch (error) {
                document.getElementById('gaussJordanResults').innerHTML = `<div class="error">Error: ${error.message}</div>`;
                document.getElementById('gaussJordanResults').style.display = 'block';
            }
        }

        function solveGaussSeidel() {
            try {
                const A = [], b = [];
                for (let i = 0; i < 3; i++) {
                    A[i] = [];
                    for (let j = 0; j < 3; j++) {
                        const value = parseFloat(document.getElementById(`gs_${i}_${j}`).value);
                        if (isNaN(value)) throw new Error(`Nilai koefisien pada baris ${i+1}, kolom ${j+1} tidak valid`);
                        A[i][j] = value;
                    }
                    const bValue = parseFloat(document.getElementById(`gs_${i}_3`).value);
                    if (isNaN(bValue)) throw new Error(`Nilai konstanta pada baris ${i+1} tidak valid`);
                    b[i] = bValue;
                }

                const tolerance = parseFloat(document.getElementById('tolerance').value) || 0.0001;
                const maxIter = parseInt(document.getElementById('maxIterations').value) || 100;

                let html = `<h3>Metode Gauss-Seidel</h3>`;
                let x = [0, 0, 0];
                let iterations = [{iter: 0, x1: 0, x2: 0, x3: 0, error: '-'}];
                let converged = false;

                for (let iter = 1; iter <= maxIter; iter++) {
                    let xOld = [...x];
                    for (let i = 0; i < 3; i++) {
                        let sum = b[i];
                        for (let j = 0; j < 3; j++) {
                            if (i !== j) sum -= A[i][j] * x[j];
                        }
                        x[i] = sum / A[i][i];
                    }

                    let maxError = Math.max(...x.map((val, i) => Math.abs(val - xOld[i])));
                    iterations.push({iter, x1: x[0], x2: x[1], x3: x[2], error: maxError});

                    if (maxError < tolerance) {
                        converged = true;
                        break;
                    }
                }

                html += `<table class="iteration-table"><tr><th>Iterasi</th><th>x₁</th><th>x₂</th><th>x₃</th><th>Error</th></tr>`;
                iterations.forEach(iter => {
                    html += `<tr><td>${iter.iter}</td><td>${typeof iter.x1 === 'number' ? iter.x1.toFixed(6) : iter.x1}</td>`;
                    html += `<td>${typeof iter.x2 === 'number' ? iter.x2.toFixed(6) : iter.x2}</td>`;
                    html += `<td>${typeof iter.x3 === 'number' ? iter.x3.toFixed(6) : iter.x3}</td>`;
                    html += `<td>${typeof iter.error === 'number' ? iter.error.toFixed(8) : iter.error}</td></tr>`;
                });
                html += `</table>`;

                if (converged) {
                    html += `<div class="solution"><h3>Solusi Konvergen!</h3>`;
                    html += `<div class="variable-result">x₁ = ${x[0].toFixed(8)}</div>`;
                    html += `<div class="variable-result">x₂ = ${x[1].toFixed(8)}</div>`;
                    html += `<div class="variable-result">x₃ = ${x[2].toFixed(8)}</div></div>`;
                } else {
                    html += `<div class="error">Solusi tidak konvergen setelah ${maxIter} iterasi.</div>`;
                }

                document.getElementById('gaussSeidelResults').innerHTML = html;
                document.getElementById('gaussSeidelResults').style.display = 'block';
            } catch (error) {
                document.getElementById('gaussSeidelResults').innerHTML = `<div class="error">Error: ${error.message}</div>`;
                document.getElementById('gaussSeidelResults').style.display = 'block';
            }
        }
    </script>
</body>
</html>
