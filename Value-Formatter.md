 
The main purpose of ValueFormatter is to give the users some mechanism to format axis labels, tooltip data and some other data which appears on the screen. 

For example the number 1000000 will be formatted to "1M".

To create valueFormatter use create method:

` valueFormatter = ValueFormatter.create(options: ValueFormatterOptions);`

 where ValueFormatterOptions is:

 interface ValueFormatterOptions {
        /** The format string to use. */
        format?: string;

        /** The data value. */
        value?: any;

        /** The data value. */
        value2?: any;

        /** The number of ticks. */
        tickCount?: any;

        /** The display unit system to use */
        displayUnitSystemType?: DisplayUnitSystemType;

        /** True if we are formatting single values in isolation (e.g. card), as opposed to multiple values with a common base (e.g. chart axes) */
        formatSingleValues?: boolean;

        /** True if we want to trim off unnecessary zeroes after the decimal and remove a space before the % symbol */
        allowFormatBeautification?: boolean;

        /** Specifies the maximum number of decimal places to show*/
        precision?: number;

        /** Specifies the column type of the data value */
        columnType?: ValueType;
    }

Example of formatter creation:

var formater = valueFormatter.create({ format: "0", value: 1e6, value2: null });

usage:

var result = formater.format(-2.4e6); // == "-2.4M"

result will be equals to "-2.4M"

Examples of formatter usage:

Numbers:

 		var formatter = valueFormatter.create({ value: 0 });

                formatter.format(0) == "0"
                formatter.format(0.5678934) == "0.5679"
                formatter.format(-0.5678934) == "-0.5679"
                formatter.format(1.234e7) == "12340000"
                formatter.format(1.12000000000007) == "1.12"

Million:
               
                var formatter = valueFormatter.create({ value: 1e6 });

                formatter.format(4.56e7) == "45.6M"
                formatter.format(4.56789123e7) == "45.68M"
                formatter.format(-3130000.567) == "-3.13M"
                formatter.format(10000) == "0.01M"
                formatter.format(100000) == "0.1M"

                or

                var format: string;
                var formatter = valueFormatter.create({ format: format, value: 300000 });

                formatter.format(300000) == "0.3M"

                or Million Whole Units

                var format: string;
                var formatter = valueFormatter.create({ format: format, value: 900000000, displayUnitSystemType: powerbi.DisplayUnitSystemType.WholeUnits });

                formatter.format(900000000) == "900M"

Billion:

                var formatter = valueFormatter.create({ value: 1e9 });

                formatter.format(4.56e10) == "45.6bn"
                formatter.format(4.56789123e10) == "45.68bn"
                formatter.format(-3130000000.567) == "-3.13bn"
                formatter.format(100000000) == "0.1bn"
                formatter.format(1000000000) == "1bn"
                formatter.format(null) == "(Blank)"

                or

                var format: string;
                var formatter = valueFormatter.create({ format: format, value: 900000000 });

                formatter.format(900000000) == "0.9bn"

                or Billion Whole Units

                var format: string;
                var formatter = valueFormatter.create({ format: format, value: 900000000000, displayUnitSystemType: powerbi.DisplayUnitSystemType.WholeUnits });

                formatter.format(900000000000) == "900bn"


Exponent format:

 				var formatter = valueFormatter.create({ format: "E", value: 1e15 });

                formatter.format(719200000000001920000000000) == "7.192000E+026"

Percentage format:

				var formatter = valueFormatter.create({ format: "0.00 %;-0.00 %;0.00 %", value: 1, allowFormatBeautification: true });

                formatter.format(0) == "0%"
                formatter.format(1) == "100%"
                formatter.format(-1) == "-100%"
                formatter.format(.54) == "54%"
                formatter.format(.543) == "54.3%"
                formatter.format(.5432) == "54.32%"
                formatter.format(.54321) == "54.32%"
                formatter.format(6.54321) == "654.32%"
                formatter.format(76.54321) == "7,654.32%"

Escaped Character format:

				var formatter = valueFormatter.create({ format: "\\$#,0.00;(\\$#,0.00);\\$#,0.00", value: 1e6 });

                formatter.format(107384391.61) == "$107.38M"
                formatter.format(-107384391.61) == "($107.38M)"

Format no custom negative:
	
				var formatter = valueFormatter.create({ format: "$#,0.00", value: 1e6 });

                formatter.format(-107384391.61) == "-$107.38M"

HundredThousand:

				

               
Dates:

				var format: string = "O";
                var minDate = new Date(2014, 10, 4, 12, 34, 56, 789);
                var maxDate = new Date(2014, 10, 9, 12, 34, 56, 789);
                var formatter = valueFormatter.create({ format: format, value: minDate, value2: maxDate, tickCount: 6 });

                formatter.format(minDate) == "Nov 04"
                formatter.format(maxDate) == "Nov 09"
                formatter.format(null) == "(Blank)"

Boolean


NaNs/nulls:

 var formatter = valueFormatter.create({ format: "0", value: 0 });
                formatter.format(Number.NaN) == "NaN"
                formatter.format(Number.NEGATIVE_INFINITY) == "-Infinity";
                formatter.format(Number.POSITIVE_INFINITY) == "+Infinity";
                formatter.format(null) == "(Blank)";



Example of tooltip info creation:

 var formatter = valueFormatter.create({ format: "0", value: 0 });

  tooltipInfo: [
                {
                    displayName: categoryColumn.source.displayName,
                    value: categoryColumn.values[categoryIndex],
                }, {
                    displayName: y.source.displayName,
                    value: formatter.format(y.values[categoryIndex])
                }
        ],

in this example all values of y category will be formatted as numbers