# Odoo 19 Thai Account Pack

Thai localization modules for Odoo 19 using OCA-first approach.

## Strategy

- ใช้ [OCA/l10n-thailand](https://github.com/OCA/l10n-thailand) branch 18.0 เป็นหลัก
- พัฒนาเพิ่มเติมเฉพาะ modules ที่ OCA ยังไม่มี
- ลดภาระการดูแล code เอง

## Modules

### From OCA (10 modules)
| Module | Description |
|--------|-------------|
| `l10n_th_account_tax` | VAT + Withholding Tax |
| `l10n_th_account_tax_report` | Tax Reports |
| `l10n_th_account_wht_cert_form` | WHT Certificate Form |
| `l10n_th_amount_to_text` | Amount to Thai Text |
| `l10n_th_base_sequence` | Base Sequence |
| `l10n_th_base_utils` | Thai Fonts + Utils |
| `l10n_th_partner` | Partner Localization |
| `l10n_th_mis_report` | MIS Report |
| `l10n_th_tier_department` | Tier Department |

### Custom (10 modules)
| Module | Description | Priority |
|--------|-------------|----------|
| `l10n_th_sequence_refactored` | Extensible sequence legends | High |
| `l10n_th_sequence_be` | Buddhist Era year | High |
| `l10n_th_sequence_branch` | Branch-based sequence | Medium |
| `l10n_th_sequence_preview` | Sequence preview | Medium |
| `l10n_th_sequence_qoy` | Quarter of Year | Low |
| `l10n_th_sequence_range_end` | Range end date | Low |
| `l10n_th_expense_tax_invoice` | Expense Tax Invoice | Medium |
| `l10n_th_expense_withholding_tax` | Expense WHT | Medium |
| `l10n_th_company_novat` | Company VAT/NOVAT | Low |
| `l10n_th_promptpay` | PromptPay Payment | Medium |

## Quick Start

```bash
# Clone repository
git clone https://github.com/YOUR_USERNAME/docker-thaiacc.git
cd docker-thaiacc

# Initialize submodules
git submodule update --init --recursive

# Start services
docker-compose up -d

# Access Odoo
open http://localhost:8069
```

## Project Structure

```
docker-thaiacc/
├── docker-compose.yml
├── .env.example
├── config/
│   └── odoo.conf
├── oca-addons/              # OCA modules (git submodules)
│   ├── l10n-thailand/
│   └── partner-contact/
├── custom-addons/           # Custom modules
├── ThaiACC/                 # Archive - original modules
├── PLAN.md                  # Development plan
├── CLAUDE.md                # AI assistant guide
└── README.md
```

## Development

See [PLAN.md](PLAN.md) for detailed development phases.

## License

AGPL-3.0

## Credits

- [OCA/l10n-thailand](https://github.com/OCA/l10n-thailand) - Main Thai localization
- [Ecosoft](https://ecosoft.co.th) - Original module development (kittiu)
