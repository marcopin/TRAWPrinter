# TRAWPrinter - Direct Text Printing

**Delphi component for RAW or direct printing.**

## Introduction

Trying to answer the problem of direct text mode printing (*raw/direct text printing*) which was discussed on the Delphindo mailing list a few days ago, I tried to provide a solution in the form of a Delphi component. This component is called **TRAWPrinter**. Initially a component created by Przemyslaw Jankowski (v.1.0) contained only the basic functions of text printing in win32. Then I developed it in such a way that it can be used more easily and supports printing to *IBM Passbook* and *Epson LX* series printers *built-in*. Actually, I have made and used this component since 2002 to print receipts to *IBM Passbook* printers (v.1.2). Then a few days ago I added printing support to the *Epson LX* series in this v.1.5.

Unlike the common text printing solutions which use text to be sent directly to the printer port (`LPT1:`), TRAWPrinter sends text to the printer *spooler* in Windows. Access to the printer *spooler* from Delphi has been provided via the WinSpool unit (default unit). The advantage of this method is that the printer we are aiming for does not have to be installed on the `LPT1:` port, even printers installed on other ports (serial, USB, etc.) can still be accessed. Even printers that are installed on the network (*shared printer*) can also be accessed in the same way. Sending data to the intended printer is the job of the *spooler* printer. So that applications on 2 computers (or more) can use 1 printer together, by using *printer sharing*. Interesting right? :)

## Usage

The use of this component is very easy and similar to the use of the Printers unit. Here is an example of usage in the demo application:

```pascal
procedure TForm1.Button1Click(Sender: TObject);
begin
  // empty printer name would print to default printer
  RAWPrinter1.PrinterName := '';

  RAWPrinter1.BeginDoc;  // start printing
  RAWPrinter1.WriteList(Memo1.Lines, true);  // print memo text
  RAWPrinter1.EndDoc;  // stop printing
end;
```

Adapun isi dari TMemo1 adalah sebagai berikut:

```html
<roman>roman</roman>
<courier>courier</courier>
<b>bold</b>
<i>italic</i>
<u>underline</u>
<s>strike</s>
<small>small</small>
normal
<big>big</big>
<double>double</double>
<sub>subtext</sub>
<super>supertext</super>
<right>right-aligned</right>
<center>centered</center>
<left>left-aligned</left>
```

And the result of printing it on the *Epson LX-300+* printer is...

![](https://github.com/marcopin/TRAWPrinter/blob/master/output.png "TRawPrinter output result")

Look at the contents of the memo and compare it with the printing results! :) That's right… TRAWPrinter v.1.5 supports several basic HTML *tags* so that the process of compiling the text to be printed becomes easier. You don't need to bother memorizing the ESC codes for each type of text format. This HTML *tag* feature can only be used for the `WriteList()` method, while other methods must be used as follows:

```pascal
  // font settings
  RAWPrinter1.FontName  := rfnCourier;
  RAWPrinter1.FontPitch := rfpCondensed;
  RAWPrinter1.FontStyle := [];
  // print text and move to next line
  RAWPrinter1.WriteLn('regular condensed courier');

  // font settings
  RAWPrinter1.FontName  := rfnRoman;
  RAWPrinter1.FontPitch := rfpExpanded;
  RAWPrinter1.FontStyle := [rfsBold];
  // print text and keep on current line
  RAWPrinter1.Write('bold expanded roman');
```

*See*… this method isn't too difficult either, and you still don't need to memorize ESC codes. *Property2* The font setting from TRAWPrinter is not much different from the TFont property. Indeed, this method is not as easy as using the `WriteList()` method above. But sometimes -in certain situations- we need more complex customization than the `WriteList()` method is capable of.

## Furthermore

Those are not the only features offered by TRAWPrinter, there are many more… such as columnar printing, margin settings for non-standard paper, the option to hold the paper after printing, and so on. And if you really feel it's necessary, you can also change the ESC codes used by components, for example for other types of printers besides Epson and IBM. You simply create an instance of this class and redefine the ESC code according to your printer.

For more details, please study *source code* of this component.

Good luck and hopefully useful.

-Bee
