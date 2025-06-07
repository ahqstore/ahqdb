# AHQ Database Installer Specification

This repository describes the installer spec of **AHQ Store**'s one of **the most flexible** application installation method.

The installer specification **is a zip file with the following** structure:

<table>
  <tr>
    <th>Windows</th>
    <th>Linux</th>
  </tr>
  <tr>
    <td>
      <pre>
(ZIP File)
install.ahqdb
├── dist/
│   └── (files to extract)
├── install.ps1
├── uninstall.ps1
├── update.ps1
├── link.toml
└── .build
      </pre>
    </td>
    <td>
      <pre>
(ZIP File)
install.ahqdb
├── dist/
│   └── (files to extract)
├── install.sh
├── uninstall.sh
├── update.sh
├── link.toml
└── .build
      </pre>
    </td>
  </tr>
</table>
