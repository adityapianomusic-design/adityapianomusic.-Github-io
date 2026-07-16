# <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Attendance & Salary Slip Generator</title>
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- html2pdf Library for generating PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">

    <div class="max-w-4xl mx-auto bg-white rounded-lg shadow-lg p-6 grid grid-cols-1 md:grid-cols-2 gap-8">
        
        <!-- Left Side: Inputs -->
        <div>
            <h2 class="text-2xl font-bold text-gray-800 mb-6">Employee Details & Attendance</h2>
            
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700">Employee Name</label>
                    <input type="text" id="empName" placeholder="Rahul Kumar" class="mt-1 block w-full p-2 border border-gray-300 rounded shadow-sm focus:ring-blue-500 focus:border-blue-500">
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700">Monthly Base Salary (₹)</label>
                    <input type="number" id="baseSalary" placeholder="30000" class="mt-1 block w-full p-2 border border-gray-300 rounded shadow-sm">
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700">Total Working Days in Month</label>
                    <input type="number" id="totalDays" value="30" class="mt-1 block w-full p-2 border border-gray-300 rounded shadow-sm">
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700">Days Present</label>
                    <input type="number" id="presentDays" placeholder="28" class="mt-1 block w-full p-2 border border-gray-300 rounded shadow-sm">
                </div>
                
                <button onclick="calculateSalary()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition">
                    Calculate & Preview Slip
                </button>
            </div>
        </div>

        <!-- Right Side: Live Salary Slip Preview -->
        <div class="border-t md:border-t-0 md:border-l pt-6 md:pt-0 md:pl-8">
            <h2 class="text-2xl font-bold text-gray-800 mb-6">Salary Slip Preview</h2>
            
            <!-- This Div will be converted to PDF -->
            <div id="salarySlip" class="bg-white p-6 border border-gray-200 rounded shadow-inner text-sm space-y-4">
                <div class="text-center border-b pb-4">
                    <h3 class="text-xl font-bold uppercase tracking-wider">Company Name Pvt. Ltd.</h3>
                    <p class="text-gray-500 text-xs">Monthly Salary Statement</p>
                </div>
                
                <div class="grid grid-cols-2 gap-2 text-gray-700">
                    <div><strong>Name:</strong> <span id="slipName">---</span></div>
                    <div><strong>Month:</strong> <span id="slipMonth">July 2026</span></div>
                    <div><strong>Total Days:</strong> <span id="slipTotalDays">0</span></div>
                    <div><strong>Present Days:</strong> <span id="slipPresent">0</span></div>
                </div>
                
                <table class="w-full text-left border-collapse mt-4">
                    <thead>
                        <tr class="border-b bg-gray-50">
                            <th class="py-2">Description</th>
                            <th class="py-2 text-right">Amount (₹)</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr class="border-b">
                            <td class="py-2">Base Monthly Salary</td>
                            <td class="py-2 text-right" id="slipBase">₹0.00</td>
                        </tr>
                        <tr class="border-b text-red-600">
                            <td class="py-2">LWP (Leave Without Pay) Deductions</td>
                            <td class="py-2 text-right" id="slipDeduction">-₹0.00</td>
                        </tr>
                        <tr class="font-bold text-lg text-green-700">
                            <td class="py-3">Net Salary Payable</td>
                            <td class="py-3 text-right" id="slipNet">₹0.00</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <!-- Download PDF Button -->
            <button onclick="downloadPDF()" class="w-full mt-4 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded transition">
                Download Salary Slip (PDF)
            </button>
        </div>

    </div>

    <script>
        function calculateSalary() {
            // Fetch inputs
            const name = document.getElementById('empName').value || "Employee";
            const baseSalary = parseFloat(document.getElementById('baseSalary').value) || 0;
            const totalDays = parseInt(document.getElementById('totalDays').value) || 30;
            const presentDays = parseInt(document.getElementById('presentDays').value) || 0;

            // Calculations
            const perDaySalary = baseSalary / totalDays;
            const absentDays = totalDays - presentDays;
            const deduction = perDaySalary * absentDays;
            const netSalary = baseSalary - deduction;

            // Update Slip UI
            document.getElementById('slipName').innerText = name;
            document.getElementById('slipTotalDays').innerText = totalDays;
            document.getElementById('slipPresent').innerText = presentDays;
            document.getElementById('slipBase').innerText = "₹" + baseSalary.toFixed(2);
            document.getElementById('slipDeduction').innerText = "-₹" + deduction.toFixed(2);
            document.getElementById('slipNet').innerText = "₹" + netSalary.toFixed(2);
        }

        function downloadPDF() {
            const element = document.getElementById('salarySlip');
            const name = document.getElementById('empName').value || "Employee";
            
            const options = {
                margin: 1,
                filename: `${name}_SalarySlip.pdf`,
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2 },
                jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
            };

            // Generate PDF
            html2pdf().set(options).from(element).save();
        }
    </script>
</body>
</html>
