# FormRestrictons
A simple **C# utility class** to handle and validate keyboard input in Windows Forms applications.  
It provides methods to block invalid keys, prevent double spaces or dashes, and validate input using regular expressions.

---

## ✨ Features

- Block invalid characters in real-time
- Allow only letters, numbers, or specific patterns
- Restrict decimal inputs with configurable integer and decimal digits
- Prevent double spaces or double dashes
- Validate input against custom regex patterns

---

## 📂 Example

```csharp
// Example: Validate that the user input matches a 3-name pattern
Keyboard.ValidateKeys(inputTextBox.Text, e, Keyboard.regex3Names);



internal class Keyboard
{
    public static string regex3Names = @"^([a-zA-Z\-]+( [a-zA-Z\-]+){0,2})?$";
    public static string regex2Surnames = @"^([a-zA-Z\-]+( [a-zA-Z\-]+){0,2})?$";
    public static string regexInitialChar = "^[a-zA-Z]+$";
    public static string regexPensionFund = "^[a-zA-Z]+$";
    public static string regexInt = "^[0-9]+$";
    public static string regexCheckDigit = "^[0-9kK]+$";
    public static string regexSubject = @"^([a-zA-Z0-9\-]+( [a-zA-Z0-9\-]+){0,5})?$";
    public static string regexSection = "^[a-zA-Z0-9]{0,3}$";
    public static string regexGrade = "^([0-9]+(,[0-9])$";
    public static string regexUser = "^[a-zA-Z0-9._-]+$";


    public static void BlockComboBox(KeyPressEventArgs e)
    {
        if (!char.IsControl(e.KeyChar))
        {
            e.Handled = true;
        }
    }

    public static void BlockSpace(KeyPressEventArgs e)
    {
        if (e.KeyChar == (char)Keys.Space)
        {
            e.Handled = true;
        }
    }

    public static void BlockDoubleSpace(string input, KeyPressEventArgs e)
    {
        if (e.KeyChar == (char)Keys.Space && input.Length > 0 && input.Last() == ' ')
        {
            e.Handled = true;
        }
    }

    public static void BlockDoubleDash(string input, KeyPressEventArgs e)
    {
        if (e.KeyChar == '-' && input.Length > 0 && input.Last() == '-')
        {
            e.Handled = true;
        }
    }

    public static void FirstChar(string input, KeyPressEventArgs e)
    {
        if (!char.IsLetter(e.KeyChar) && e.KeyChar != (char)Keys.Back && input.Length == 0)
        {
            e.Handled = true;
        }
    }

    // RESTRICTION FOR DECIMAL VALUES LESS THAN 100
    public static void RestrictNumericDecimal(string input, int integerDigits, int decimalDigits, KeyPressEventArgs e)
    {
        int nDigits = integerDigits + decimalDigits + 1;
        char separator = ',';

        if (!char.IsDigit(e.KeyChar) && e.KeyChar != separator && e.KeyChar != (char)Keys.Back)
        {
            e.Handled = true;
        }

        if (e.KeyChar == separator && input.Contains(separator))
        {
            e.Handled = true;
        }

        if (input.Length >= nDigits
            && e.KeyChar != (char)Keys.Back)
        {
            e.Handled = true;
        }
    }

    public static void ValidateKeys(string input, KeyPressEventArgs e, string regex)
    {
        string charInput = input + e.KeyChar;

        if (!Regex.IsMatch(charInput, regex) && e.KeyChar != (char)Keys.Back && e.KeyChar != (char)Keys.Space)
        {
            e.Handled = true;
        }
    }

}
