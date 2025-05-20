<!DOCTYPE html>
<html lang="ro">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Comandă Semințe</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: left; }
    input[type="number"] { width: 60px; }
    tfoot td { font-weight: bold; }
    button { margin-top: 20px; padding: 10px 20px; font-size: 16px; }
  </style>
</head>
<body>

  <h1>Comandă Semințe</h1>

  <table id="tabelSeminte">
    <thead>
      <tr>
        <th>Produs</th>
        <th>Preț (lei)</th>
        <th>Cantitate</th>
        <th>Total (lei)</th>
      </tr>
    </thead>
    <tbody id="corpTabel"></tbody>
    <tfoot>
      <tr>
        <td colspan="3">Total general</td>
        <td id="totalGeneral">0.00</td>
      </tr>
    </tfoot>
  </table>

  <button onclick="genereazaFisier()">Trimite Comandă</button>

  <script>
    const produse = [
      ["Seminte albe 25g", 0.75], ["Seminte albe 50g New", 2.30], ["Seminte albe 80gr NEW", 3.85],
      ["Seminte albe 120gr", 5.95], ["Seminte pestrite 25g", 0.75], ["Seminte pestrite 50gr New", 2.30],
      ["Seminte pestrite 90gr New", 3.80], ["Seminte pestrite 165gr", 5.95], ["Seminte 40gr", 0.75],
      ["Seminte 100gr", 2.65], ["Seminte 130gr", 3.80], ["Seminte 200gr NEW", 5.30],
      ["Seminte sare 40gr", 0.75], ["Seminte sare 100gr", 2.65], ["Seminte sare 130gr", 3.80],
      ["Seminte sare 200gr Nou", 5.30], ["Miez 30gr", 0.77], ["Miez 55gr", 1.59],
      ["Seminte dov. 40g", 2.30], ["Seminte dov. 80g NEW", 3.80], ["Seminte dov. 110g", 5.30],
      ["Party Mix 'Banzai' 50gr", 2.30], ["Mix 30gr NEW", 1.17], ["Mix 60gr", 2.30],
      ["Mix 110gr New", 3.85], ["Porumb prajit 30gr New", 1.17], ["Porumb 60gr NEW", 2.30],
      ["Porumb 110gr NEW", 3.85], ["Arahide decoj 70g", 2.30], ["Arahide decoj 140g", 3.85],
      ["Arahide coaja 70gr", 2.30], ["Arahide coaja 140gr", 3.85], ["Caju 60gr", 6.90],
      ["Alune 70gr", 6.90], ["Migdale 70gr NEW", 6.90], ["Fistic 30gr", 4.60],
      ["Fistic 50gr New", 7.75], ["Fistic 120gr New", 16.90]
    ];

    const corpTabel = document.getElementById("corpTabel");
    const totalGeneral = document.getElementById("totalGeneral");

    produse.forEach(([nume, pret], index) => {
      const rand = document.createElement("tr");

      const colNume = document.createElement("td");
      colNume.textContent = nume;

      const colPret = document.createElement("td");
      colPret.textContent = pret.toFixed(2);

      const colCantitate = document.createElement("td");
      const inputCantitate = document.createElement("input");
      inputCantitate.type = "number";
      inputCantitate.min = "0";
      inputCantitate.value = "0";
      inputCantitate.addEventListener("input", calculeazaTotaluri);
      colCantitate.appendChild(inputCantitate);

      const colTotal = document.createElement("td");
      colTotal.textContent = "0.00";

      rand.appendChild(colNume);
      rand.appendChild(colPret);
      rand.appendChild(colCantitate);
      rand.appendChild(colTotal);

      corpTabel.appendChild(rand);
    });

    function calculeazaTotaluri() {
      let total = 0;
      [...corpTabel.rows].forEach((rand, index) => {
        const pret = produse[index][1];
        const cant = parseInt(rand.cells[2].firstChild.value) || 0;
        const totalProdus = pret * cant;
        rand.cells[3].textContent = totalProdus.toFixed(2);
        total += totalProdus;
      });
      totalGeneral.textContent = total.toFixed(2);
    }

    function genereazaFisier() {
      let text = "Comandă semințe:\n\n";
      let total = 0;

      [...corpTabel.rows].forEach((rand, index) => {
        const nume = produse[index][0];
        const pret = produse[index][1];
        const cant = parseInt(rand.cells[2].firstChild.value) || 0;
        const totalProdus = pret * cant;
        if (cant > 0) {
          text += `${nume} - ${cant} buc x ${pret.toFixed(2)} lei = ${totalProdus.toFixed(2)} lei\n`;
          total += totalProdus;
        }
      });

      text += `\nTotal general: ${total.toFixed(2)} lei`;

      const blob = new Blob([text], { type: "text/plain" });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "comanda_semințe.txt";
      link.click();
    }
  </script>

</body>
</html>
