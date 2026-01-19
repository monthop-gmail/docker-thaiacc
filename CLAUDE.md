# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Thai Account Pack for Odoo 19 using OCA-first approach.

**Strategy**: ใช้ OCA/l10n-thailand 18.0 เป็นหลัก + พัฒนาเพิ่มเติมเฉพาะที่จำเป็น
**Target Version**: Odoo 19
**License**: AGPL-3
**Main Source**: https://github.com/OCA/l10n-thailand (branch 18.0)

## Development Environment

### Docker Setup (Planned)
```bash
# Start Odoo 19 with PostgreSQL
docker-compose up -d

# View logs
docker-compose logs -f odoo

# Restart Odoo after code changes
docker-compose restart odoo
```

### Running Tests
```bash
# Run tests for a specific module inside Docker container
docker-compose exec odoo odoo -d testdb --test-enable --stop-after-init -i l10n_th_tax_invoice

# Run all tests for Thai modules
docker-compose exec odoo odoo -d testdb --test-enable --stop-after-init -i l10n_th_amount_to_text,l10n_th_tax_invoice,l10n_th_withholding_tax
```

### Update Module After Changes
```bash
docker-compose exec odoo odoo -d odoo -u l10n_th_tax_invoice --stop-after-init
```

## Module Architecture

### OCA Modules (ใช้จาก GitHub - ไม่ต้องดูแลเอง)

```
oca-addons/l10n-thailand/       # git submodule
├── l10n_th_account_tax         # VAT + WHT (รวม tax_invoice + withholding_tax)
├── l10n_th_account_tax_report  # Tax Reports
├── l10n_th_account_wht_cert_form
├── l10n_th_amount_to_text
├── l10n_th_base_sequence
├── l10n_th_base_utils          # Thai Fonts + Utils
├── l10n_th_partner
├── l10n_th_mis_report
├── l10n_th_tier_department
└── l10n_th_tier_department_demo

oca-addons/partner-contact/     # git submodule
├── partner_company_type
└── partner_firstname
```

### Custom Modules (ต้องพัฒนา/ดูแลเอง)

```
custom-addons/
├── l10n_th_sequence_refactored # Extensible sequence legends
├── l10n_th_sequence_be         # Buddhist Era year
├── l10n_th_sequence_branch     # Branch-based sequence
├── l10n_th_sequence_preview    # Sequence preview
├── l10n_th_sequence_qoy        # Quarter of Year
├── l10n_th_sequence_range_end  # Range end date
├── l10n_th_expense_tax_invoice # Expense Tax Invoice
├── l10n_th_expense_withholding_tax
├── l10n_th_company_novat
└── l10n_th_promptpay
```

### Sequence System (Custom)

The sequence modules extend `ir.sequence` with additional placeholders:
- `%(be_year)s` - Buddhist Era year (l10n_th_sequence_be)
- `%(qoy)s` - Quarter of year (l10n_th_sequence_qoy)
- `%(branch)s` - Branch code (l10n_th_sequence_branch)

## OCA Repositories

| Repository | Branch | URL |
|------------|--------|-----|
| l10n-thailand | 18.0 | https://github.com/OCA/l10n-thailand |
| partner-contact | 18.0 | https://github.com/OCA/partner-contact |
| server-ux | 18.0 | https://github.com/OCA/server-ux |

## Git Submodules Setup

```bash
git submodule add -b 18.0 https://github.com/OCA/l10n-thailand.git oca-addons/l10n-thailand
git submodule add -b 18.0 https://github.com/OCA/partner-contact.git oca-addons/partner-contact
git submodule update --init --recursive
```

## Odoo 19 Migration Notes

OCA 18.0 modules should be mostly compatible with Odoo 19. Check:
- `__manifest__.py` version format: `19.0.x.x.x`
- `<tree>` vs `<list>` view tags
- OWL component syntax changes
- Asset bundle definitions
