# CalculatorTest

## This is the text of Calculator from open.163.com

//  ViewController.swift
//  CalculatorTest
//
//  Created by babykang on 15/4/11.
//  Copyright (c) 2015年 Fiona. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var numberShow: UILabel!
    
    var userInTheMiddleOfTyping = false
   
    @IBAction func apendDigit(sender: UIButton) {
        var digit = sender.currentTitle!
        if userInTheMiddleOfTyping{
            numberShow.text = numberShow.text! + digit
        }else {
            numberShow.text = digit
            userInTheMiddleOfTyping = true
        }
    }
    
    
    @IBAction func operate(sender: UIButton) {
        let operation = sender.currentTitle!
        if userInTheMiddleOfTyping {
            enter()
        }
        switch operation{
        case "✖️":preformOperation {$0*$1} //({(op1, op2)in return op1 * op2})
            
        case "➗":preformOperation {$0/$1} //({(op1,op2) in return op1 / op2})
            
        case "➕":preformOperation {$0+$1} //({(op1,op2) in return op1 + op2})
            
        case "➖":preformOperation {$0-$1}  //({(op1,op2) in return op1 - op2})
            
        case "√": preformOperation {sqrt($0)}
            
        case "%":preformOperation {$0%$1}
            
        default:break
            
        }
    }
    
    func preformOperation(operation:(Double,Double)->Double){
        if operandStack.count >= 2{
            numberShowValue = operation(operandStack.removeLast(), operandStack.removeLast())
            enter()
        }
    }
    
    func preformOperation(operation:(Double)->Double){
        if operandStack.count >= 2{
            numberShowValue = operation(operandStack.removeLast())
            enter()
        }
    }    //在swift里，定义两个名字一样的函数是可以的，因为两个函数的参数不同，在运行时，系统会自己选择适合自己的函数。

    
   /* func multitype (op1:Double, op2:Double)->Double {
        return op1*op2
    }
    
    func divide (op1:Double, op2:Double)->Double {
        return op1 / op2
    }
    func plus (op1:Double, op2:Double)->Double {
        return op1 + op2
    }
    func subtraction (op1:Double, op2:Double)->Double {
        return op1 - op2
    }
    */


    
    var operandStack = Array<Double>()
    
    @IBAction func enter() {
        userInTheMiddleOfTyping = false
        operandStack.append(numberShowValue)
        println("operandStack = \(operandStack)")
    }
    
    var numberShowValue : Double{
        get {
            return NSNumberFormatter().numberFromString(numberShow.text!)!.doubleValue
        }
        set{
            numberShow.text = "\(newValue)"
            userInTheMiddleOfTyping = false
        }
        
    }
    
    
    @IBAction func startButton(sender: UIButton) {
        numberShow.text = "0"
    }
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}


