import UIKit

// ------- DESCRIPTION ------- //
/* this program calculates provincial (for MB, ON, QC, and BC) and federal tax deductions for a given income. It takes into account basic personal amount for both federal and provincial taxes. Other non-refundable credits are not taken into consideration here at this time. */

// -------- ENTER INCOME HERE ------ //
let income = 100000.0
// -------- ---------------- ------ //

// ------- FEDERAL TAX CALCULATION ------- //
let federalTaxDeduction = FederalTax().getTaxAmount(income: income)

// ------- PROVINCIAL TAX CALCULATIONS -------- //
//each line calculates the income tax for a separate province, and the line directly beneath it prints out a summary for deductions on both federal and provincial level.
let mbTaxDeduction = MBIncomeTax().getTaxAmount(income: income)
print("After federal and provincial taxes are deducted, a Manitoba resident with a gross income of $\(income) will have $\(income-federalTaxDeduction-mbTaxDeduction) in net income remaining.")
let onTaxDeduction = ONIncomeTax().getTaxAmount(income: income)
print("After federal and provincial taxes are deducted, an Ontario resident with a gross income of $\(income) will have $\(income-federalTaxDeduction-onTaxDeduction) in net income remaining.")
let bcTaxDeduction = BCIncomeTax().getTaxAmount(income: income)
print("After federal and provincial taxes are deducted, a BC resident with a gross income of $\(income) will have $\(income-federalTaxDeduction-bcTaxDeduction) in net income remaining.")
let qcTaxDeduction = QCIncomeTax().getTaxAmount(income: income)
print("After federal and provincial taxes are deducted, a Quebec resident with a gross income of $\(income) will have $\(income-federalTaxDeduction-qcTaxDeduction) in net income remaining.")


// ------ SUPERCLASS ----- //
class IncomeTax {
    var taxBrackets: [TaxBracket] = [] //tax brackets
    var highestBracketRate = 0.0 //the tax rate for income above the highest bracket's maximum
    var basicPersonalAmount = 0.0 //a portion of income that isn't taxed
    var name = "" //name of government
    
    //getTaxAmount calculates the income tax deducted based on the income and bracket information
    func getTaxAmount(income: Double) -> Double {
        var remainder = income //the remaining income that will be taxed (passed from one bracket to next)
        var totalTax = 0.0 //the total amount of tax paid (sum of tax paid in each applicable bracket)
        var bracket = 0 //first bracket
        
        //if there is income that exceeds the basic personal amount, it will be taxed
        if income > basicPersonalAmount {
            remainder = income - basicPersonalAmount
            while remainder > 0  && bracket < taxBrackets.count {
                let currentBracket = taxBrackets[bracket] //start with lowest bracket
                totalTax += currentBracket.calculateTaxPaid(income: remainder)
                remainder -= currentBracket.bracketMax //deduct the bracket's maximum from the income, the remainder will be taxed in the next bracket
                bracket += 1 //move to next highest bracket with remaining income
            }
            
            //if there is remaining income above the fourth bracket, all of it is taxed at the highest bracket rate
            if remainder > 0 {
                totalTax += remainder * highestBracketRate
            }
        }
        printSummary(income: income, totalTax: totalTax) //print summary after calculating
        return round(totalTax)
    }
    
    //prints the deduction summary
    private func printSummary(income: Double, totalTax: Double) {
        print("\n\nBy the \(name), an income of $\(income) will be have $\(round(totalTax)) deducted as annual taxes.")
    }
}

// this class stores information for an individual tax bracket, and calculates the tax paid based on amount of income that falls within the bracket
class TaxBracket {
    private var maximum: Double //the maximum income that can be taxed in this bracket
    private var taxRate: Double //tax rate for this bracket
    
    init(maximum: Double, taxRate: Double) {
        self.maximum = maximum
        self.taxRate = taxRate
    }
    
    var bracketMax: Double {
        return maximum
    }
    
    var rate: Double {
        return taxRate
    }
    
    //returns the amount of taxes paid for this tax bracket
    //usually, a "remainder" would be provided as income -- the amount of income remaining after the previous tax bracket has been processed
    func calculateTaxPaid(income: Double) -> Double {
        //if the income is less than or equal to the tax bracket amount, return the income multiplied by the tax rate
        if income <= maximum {
            return income * taxRate
        } else { //if the income is higher than the tax bracket amount, return the full tax bracket amount multiplied by the tax rate
            return maximum * taxRate
        }
    }
}


// ------ SUBCLASSES ------ //

//This class contains tax brackets for federal income tax
class FederalTax: IncomeTax {
    override init() {
        super.init()
        //the tax brackets which income is taxed at federal level
        taxBrackets.append(TaxBracket(maximum: 50197.0, taxRate: 0.15)) //first bracket
        taxBrackets.append(TaxBracket(maximum: 100392, taxRate: 0.20)) //second bracket
        taxBrackets.append(TaxBracket(maximum: 155625, taxRate: 0.26)) //third bracket
        taxBrackets.append(TaxBracket(maximum: 221708, taxRate: 0.29)) //fourth bracket
        basicPersonalAmount = (income <= 150473.0) ? 13229.0 : 12298.0
        highestBracketRate = 0.33
        name = "Federal Government"
    }
}


//MBIncomeTax contains tax brackets for Manitoba income tax
class MBIncomeTax: IncomeTax {
    override init() {
        super.init()
        taxBrackets.append(TaxBracket(maximum: 34431.0, taxRate: 0.108)) //first bracket
        taxBrackets.append(TaxBracket(maximum: 74416.0, taxRate: 0.1275)) //second bracket
        basicPersonalAmount = 10145.0
        highestBracketRate = 0.174
        name = "Manitoba Government"
    }
    
}

//ONIncomeTax contains tax brackets for Ontario income tax
class ONIncomeTax: IncomeTax {
    override init() {
        super.init()
        taxBrackets.append(TaxBracket(maximum: 46226.0, taxRate: 0.0505))
        taxBrackets.append(TaxBracket(maximum: 92454.0, taxRate: 0.0915))
        taxBrackets.append(TaxBracket(maximum: 150000.0, taxRate: 0.1116))
        taxBrackets.append(TaxBracket(maximum: 220000.0, taxRate: 0.1216))
        basicPersonalAmount = 11141.0
        highestBracketRate = 0.1316
        name = "Ontario Government"
    }
}

//QCIncomeTax contains tax brackets for Quebec income tax
class QCIncomeTax: IncomeTax {
    override init() {
        super.init()
        taxBrackets.append(TaxBracket(maximum: 46295.0, taxRate: 0.15))
        taxBrackets.append(TaxBracket(maximum: 92580.0, taxRate: 0.20))
        taxBrackets.append(TaxBracket(maximum: 112665, taxRate: 0.24))
        basicPersonalAmount = 16143.0
        highestBracketRate = 0.2575
        name = "Quebec Government"
    }
}

//BCIncomeTax contains tax brackets for British Columbia income tax
class BCIncomeTax: IncomeTax {
    override init() {
        super.init()
        taxBrackets.append(TaxBracket(maximum: 43070.0, taxRate: 0.0506))
        taxBrackets.append(TaxBracket(maximum: 86141.0, taxRate: 0.077))
        taxBrackets.append(TaxBracket(maximum: 98901.0, taxRate: 0.105))
        taxBrackets.append(TaxBracket(maximum: 120094.0, taxRate: 0.1229))
        taxBrackets.append(TaxBracket(maximum: 162832.0, taxRate: 0.147))
        taxBrackets.append(TaxBracket(maximum: 227091.0, taxRate: 0.168))
        basicPersonalAmount = 11302.0
        highestBracketRate = 0.205
        name = "British Columbia Government"
    }
}
