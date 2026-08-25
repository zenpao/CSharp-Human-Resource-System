# CSharp-Human-Resource-System (HRMS)

A Windows Forms desktop app (C#) for managing employee personnel records — personal data sheets, education, and work experience — backed by a local Microsoft Access database, with login-based access.

**No packaged `/dist` executable is included — the repo instead bundles the runtime installers needed to build/run the app, plus a `run-HRMS.bat` launcher for the Debug build.**

## Description

HRMS lets a logged-in user manage a table of personnel: add new entries, view/update each person's Personal Data Sheet (PDS), Education history, and Work Experience, delete entries, and search/refresh the personnel list. Quick-access menu items open the folders used for PDS/resume, SALN, and payroll files in Windows Explorer. Payroll and Leave Credits features are present in the menu but currently show "Feature available soon."

## Features

- **Login** — validates username/password against a `tblSuperUsers` database table, greets the user by name and privilege level on success
- **Database connectivity check** on startup, with a clear error if the database file is missing/corrupt
- **Dashboard** — data grid of personnel records, with:
  - Add new personnel (opens the Personal Data Sheet form)
  - Delete a selected personnel record (with confirmation)
  - Open a selected personnel's Personal Data Sheet, Education, or Work Experience for viewing/updating
  - Search personnel and refresh the list
  - Quick-open the PDS/Resume, SALN, and Payroll folders in Windows Explorer
  - Menu placeholders for Payroll and Leave Credits ("Feature available soon")
- **Personal Data Sheet (PDS) form** — enter/edit an employee's personal information
- **Education (EDU) form** — enter/edit an employee's education history
- **Work Experience (WE) form** — enter/edit an employee's work experience history

## Tech Stack

- **C#** (.NET Framework, targetFramework `net45`)
- **Windows Forms** (WinForms)
- **Microsoft Access (`.mdb`)** database via `System.Data.OleDb` (Jet OLEDB 4.0 provider)
- **itext7** `7.1.16` and **iTextSharp** `5.5.13.2` — PDF libraries included as NuGet dependencies (for planned PDF generation/export; not yet wired into the current forms)
- `BouncyCastle`, `Portable.BouncyCastle`, `Common.Logging` — transitive dependencies of the iText packages

## Prerequisites

- Windows OS
- .NET Framework 4.5 or later — for running the built app
- Microsoft Access Database Engine / Jet OLEDB 4.0 provider (to connect to the `.mdb` database) — an installer is bundled at `components/mdac28sdk.msi`, and 32-bit/64-bit Access Database Engine installers are bundled at `engines/AccessDatabaseEngine.exe` / `engines/AccessDatabaseEngine_X64.exe`
- Microsoft Visual C++ Redistributable (2005–2019) — installer bundled at `visualc++/Microsoft Visual C++ (2005-2019).exe`
- Visual Studio (for building/editing the project)

## Installation

Clone the repository:

```bash
git clone https://github.com/paoradox/CSharp-Human-Resource-System.git
cd CSharp-Human-Resource-System
```

If needed, run the bundled installers in `components/`, `engines/`, and `visualc++/` to satisfy the database and runtime prerequisites above.

Open `HRMS/HRMS.sln` in Visual Studio and build the solution — NuGet should restore the packages listed in `packages.config` automatically.

## Usage

### Option 1: Run with the provided batch script

From the repo root, run:

```
run-HRMS.bat
```

This launches the built executable at `HRMS/HRMS/bin/Debug/HRMS.exe`.

### Option 2: Run manually

Build the solution in Visual Studio, then run the generated `HRMS.exe` directly (or run/debug from within Visual Studio).

### Workflow

1. **Log in** with a username and password (stored in the `tblSuperUsers` table).
2. From the **Dashboard**, add new personnel or select an existing record.
3. Open **Personal Information**, **Education**, or **Work Experience** to view or update that person's details.
4. Use the folder-shortcut menu items to open the PDS/Resume, SALN, or Payroll folders directly.

## Configuration

- The database connection points to `HRMS.mdb`, expected in the same folder as the built executable.
- Expects `pds`, `saln`, and `payroll` subfolders (relative to the executable) for the folder-shortcut menu items to open successfully.

## Troubleshooting

- **"Database missing or corrupt" on startup:** ensure `HRMS.mdb` is present in the same folder as `HRMS.exe`, and that the Access Database Engine is installed.
- **Database connection errors:** ensure the Microsoft Access Database Engine (Jet OLEDB 4.0) and Visual C++ Redistributable are installed — installers for both are bundled in the repo.
- **"Username or password is incorrect":** double-check credentials against the `tblSuperUsers` table.

## Project Structure

```
CSharp-Human-Resource-System/
├── HRMS/
│   ├── HRMS/
│   │   ├── Auth.cs / Auth.Designer.cs / Auth.resx           # Login form
│   │   ├── Dashboard.cs / Dashboard.Designer.cs / Dashboard.resx  # Main dashboard
│   │   ├── PDS.cs / PDS.Designer.cs / PDS.resx               # Personal Data Sheet form
│   │   ├── EDU.cs / EDU.Designer.cs / EDU.resx               # Education form
│   │   ├── WE.cs / WE.Designer.cs / WE.resx                  # Work Experience form
│   │   ├── Global.cs                                          # Shared/global variables
│   │   ├── Program.cs                                         # App entry point
│   │   ├── Properties/                                        # Assembly info, settings
│   │   ├── img/                                                # App icon and logo
│   │   ├── bin/Debug/                                          # Build output
│   │   ├── obj/                                                # Build intermediates
│   │   ├── App.config
│   │   ├── HRMS.csproj                                         # Project file
│   │   └── packages.config                                     # NuGet package references
│   ├── packages/                                                # Restored NuGet packages
│   ├── HRMS.sln                                                 # Visual Studio solution file
│   └── HRMS.suo / HRMS.v12.suo                                  # VS user options (local state)
├── components/
│   └── mdac28sdk.msi                    # MDAC/Jet installer
├── engines/
│   ├── AccessDatabaseEngine.exe          # Access Database Engine (32-bit)
│   └── AccessDatabaseEngine_X64.exe      # Access Database Engine (64-bit)
├── itext7/
│   ├── itext7.7.1.16.nupkg
│   └── INSTALL.txt
├── itextsharp5/
│   ├── itextsharp.5.5.13.2.nupkg
│   └── INSTALL.txt
├── visualc++/
│   └── Microsoft Visual C++ (2005-2019).exe
└── run-HRMS.bat                          # Launches the built executable
```

## License

Not specified.
