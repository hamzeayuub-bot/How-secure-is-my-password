# PowerShell Password Strength Checker

A simple PowerShell script that analyzes the strength of a password based on length and character complexity.

## Overview

This script was built to test password strength directly in the Windows PowerShell console. It uses a scoring system to rate passwords from 1 to 5 based on common security criteria.

The screenshot shows the script running and testing two different passwords: a strong 13-character password and a weak 3-character password.

## How It Works

The script performs the following steps:

1. **Secure Input:** Prompts the user to enter a password using `Read-Host -AsSecureString` so the password is not displayed in plain text on the screen.
2. **Conversion:** Converts the secure string back to plain text temporarily so it can be analyzed.
3. **Scoring Logic:** Checks the password against 5 criteria. Each criterion passed adds 1 point to the score.
4. **Rating:** Displays the final score (out of 5) and assigns a strength rating (STRONG, MEDIUM, or WEAK).

## Scoring Criteria

The script awards 1 point for each of the following:

| Criteria | Description |
|---|---|
| **Length** | Password is 12 or more characters |
| **Uppercase** | Contains at least one uppercase letter (A-Z) |
| **Lowercase** | Contains at least one lowercase letter (a-z) |
| **Numbers** | Contains at least one number (0-9) |
| **Special Characters** | Contains at least one non-alphanumeric character (e.g., `!`, `@`, `#`, `$`) |

## Strength Ratings

Based on the final score, the script outputs one of the following:

- **Score 5:** `STRONG` (Green)
- **Score 3–4:** `MEDIUM` (Yellow)
- **Score 0–2:** `WEAK` (Red)

## Usage Example (From the Screenshot)

### Test 1: Strong Password
- **Length:** 13 characters
- **Score:** 5 / 5
- **Result:** `Strength: STRONG`
- **Why:** The password meets all 5 criteria (length, uppercase, lowercase, numbers, and special characters).

### Test 2: Weak Password
- **Length:** 3 characters
- **Score:** 1 / 5
- **Result:** `Strength: WEAK`
- **Why:** The password is too short and likely only meets one of the criteria (probably lowercase or numbers).

## The Code

```powershell
$password = Read-Host "Enter a password to test" -AsSecureString
$plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($password))

$score = 0

if ($plainPassword.Length -ge 12) { $score++ }
if ($plainPassword -match "[A-Z]") { $score++ }
if ($plainPassword -match "[a-z]") { $score++ }
if ($plainPassword -match "[0-9]") { $score++ }
if ($plainPassword -match "[^a-zA-Z0-9]") { $score++ }

Write-Host "`n--- Password Analysis ---" -ForegroundColor Cyan
Write-Host "Length: $($plainPassword.Length) characters"
Write-Host "Password Score: $score / 5"

if ($score -eq 5) {
    Write-Host "Strength: STRONG" -ForegroundColor Green
} elseif ($score -ge 3) {
    Write-Host "Strength: MEDIUM" -ForegroundColor Yellow
} else {
    Write-Host "Strength: WEAK" -ForegroundColor Red
}
