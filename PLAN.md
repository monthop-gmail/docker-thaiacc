# Odoo 19 Thai Account Pack - Development Plan

## Project Overview
- **Project**: Thai Account Pack Modules for Odoo 19
- **Strategy**: ใช้ OCA/l10n-thailand 18.0 เป็นหลัก + พัฒนาเพิ่มเติมเฉพาะที่จำเป็น
- **Source**: https://github.com/OCA/l10n-thailand (branch 18.0)
- **Testing**: Docker Compose environment

---

## Strategy: OCA-First Approach

### หลักการ
1. ใช้ modules จาก OCA 18.0 ให้มากที่สุด (ลดภาระดูแล code)
2. Fork/migrate เฉพาะ modules ที่ OCA ยังไม่มี
3. Contribute กลับไป OCA เมื่อพร้อม

---

## OCA 18.0 Modules (ใช้ได้เลย - 10 modules)

| Module | Version | Description | แทน ThaiACC เดิม |
|--------|---------|-------------|------------------|
| `l10n_th_account_tax` | 18.0.1.5.2 | VAT + Withholding Tax | tax_invoice, withholding_tax, wht_cert |
| `l10n_th_account_tax_report` | 18.0.1.0.0 | Tax Reports | tax_report, wht_report |
| `l10n_th_account_wht_cert_form` | 18.0.1.0.0 | WHT Certificate Form | wht_cert_form |
| `l10n_th_amount_to_text` | 18.0.2.0.0 | Amount to Thai Text | amount_to_text |
| `l10n_th_base_sequence` | 18.0.1.0.0 | Base Sequence | base_sequence |
| `l10n_th_base_utils` | 18.0.2.0.0 | Thai Fonts + Utils | fonts, google_fonts |
| `l10n_th_partner` | 18.0.1.1.0 | Partner Localization | partner |
| `l10n_th_mis_report` | 18.0.1.0.0 | MIS Report | (new) |
| `l10n_th_tier_department` | 18.0.1.0.0 | Tier Department | (new) |
| `l10n_th_tier_department_demo` | 18.0.1.0.0 | Demo Data | (new) |

---

## Modules ต้องพัฒนาเอง (ThaiACC - ยังไม่มีใน OCA 18.0)

### A. Sequence Extensions (6 modules)
| Module | Description | Priority |
|--------|-------------|----------|
| `l10n_th_sequence_refactored` | Extensible sequence legends | High |
| `l10n_th_sequence_be` | Buddhist Era year | High |
| `l10n_th_sequence_preview` | Sequence preview | Medium |
| `l10n_th_sequence_qoy` | Quarter of Year | Low |
| `l10n_th_sequence_branch` | Branch-based sequence | Medium |
| `l10n_th_sequence_range_end` | Range end date | Low |

### B. Expense Extensions (2 modules)
| Module | Description | Priority |
|--------|-------------|----------|
| `l10n_th_expense_tax_invoice` | Expense Tax Invoice | Medium |
| `l10n_th_expense_withholding_tax` | Expense WHT | Medium |

### C. Other (2 modules)
| Module | Description | Priority |
|--------|-------------|----------|
| `l10n_th_company_novat` | Company VAT/NOVAT | Low |
| `l10n_th_promptpay` | PromptPay Payment | Medium |

---

## Development Phases (Revised)

### Phase 1: Docker Environment Setup
- [ ] สร้าง `docker-compose.yml` สำหรับ Odoo 19 + PostgreSQL 16
- [ ] สร้าง `.env` file สำหรับ configuration
- [ ] Clone OCA/l10n-thailand branch 18.0
- [ ] ทดสอบ Odoo 19 รันได้ปกติ

### Phase 2: Install OCA Modules
- [ ] ติดตั้ง OCA dependencies (partner_company_type, partner_firstname)
- [ ] ติดตั้ง `l10n_th_base_utils` (fonts)
- [ ] ติดตั้ง `l10n_th_amount_to_text`
- [ ] ติดตั้ง `l10n_th_base_sequence`
- [ ] ติดตั้ง `l10n_th_partner`
- [ ] ติดตั้ง `l10n_th_account_tax`
- [ ] ติดตั้ง `l10n_th_account_tax_report`
- [ ] ติดตั้ง `l10n_th_account_wht_cert_form`
- [ ] ทดสอบ functional ทั้งหมด

### Phase 3: Migrate Custom Modules (High Priority)
- [ ] `l10n_th_sequence_refactored` - ปรับให้ compatible กับ 19.0
- [ ] `l10n_th_sequence_be` - Buddhist Era
- [ ] `l10n_th_sequence_branch` - Branch sequence

### Phase 4: Migrate Custom Modules (Medium Priority)
- [ ] `l10n_th_sequence_preview`
- [ ] `l10n_th_expense_tax_invoice`
- [ ] `l10n_th_expense_withholding_tax`
- [ ] `l10n_th_promptpay`

### Phase 5: Migrate Custom Modules (Low Priority)
- [ ] `l10n_th_sequence_qoy`
- [ ] `l10n_th_sequence_range_end`
- [ ] `l10n_th_company_novat`

### Phase 6: Testing & Documentation
- [ ] Unit tests ทุก module
- [ ] Integration tests
- [ ] Documentation

---

## Docker Compose Structure (Revised)

```
docker-thaiacc/
├── docker-compose.yml
├── .env
├── config/
│   └── odoo.conf
├── oca-addons/              # OCA modules (git submodule)
│   ├── l10n-thailand/       # git clone -b 18.0 https://github.com/OCA/l10n-thailand
│   ├── partner-contact/     # สำหรับ partner_company_type, partner_firstname
│   └── server-ux/           # สำหรับ tier_validation (ถ้าต้องการ)
├── custom-addons/           # Custom modules ที่ต้องพัฒนาเอง
│   ├── l10n_th_sequence_refactored/
│   ├── l10n_th_sequence_be/
│   ├── l10n_th_sequence_branch/
│   ├── l10n_th_promptpay/
│   └── ...
├── ThaiACC/                 # (Archive) Original modules for reference
└── PLAN.md
```

---

## Git Submodules Setup

```bash
# Add OCA repositories as submodules
git submodule add -b 18.0 https://github.com/OCA/l10n-thailand.git oca-addons/l10n-thailand
git submodule add -b 18.0 https://github.com/OCA/partner-contact.git oca-addons/partner-contact
git submodule add -b 18.0 https://github.com/OCA/server-ux.git oca-addons/server-ux

# Update submodules
git submodule update --init --recursive
```

---

## Odoo 19 Compatibility Notes

### จาก OCA 18.0 → Odoo 19
- ส่วนใหญ่ควร compatible เนื่องจาก 18.0 → 19.0 มี breaking changes น้อย
- ตรวจสอบ `__manifest__.py` version
- ตรวจสอบ deprecated APIs

### Breaking Changes to Watch
- `<tree>` vs `<list>` tag
- OWL component syntax
- Asset bundling

---

## External Dependencies

### OCA Repositories Required
| Repository | Branch | Modules |
|------------|--------|---------|
| l10n-thailand | 18.0 | All Thai modules |
| partner-contact | 18.0 | partner_company_type, partner_firstname |
| server-ux | 18.0 | tier_validation (optional) |

### Python Packages
- `num2words` - Thai text conversion

---

## Progress Tracking

| Date | Phase | Status | Notes |
|------|-------|--------|-------|
| 2026-01-19 | Phase 1 | ✅ Done | Docker setup + OCA submodules |
| 2026-01-19 | Phase 2 | ✅ Done | OCA modules installed + demo data |
| - | Phase 3 | Pending | High priority custom (sequence_be) |
| - | Phase 4 | Pending | Medium priority custom |
| - | Phase 5 | Pending | Low priority custom |
| - | Phase 6 | Pending | Testing |

---

## References

- OCA l10n-thailand: https://github.com/OCA/l10n-thailand
- OCA partner-contact: https://github.com/OCA/partner-contact
- Odoo 19 Release Notes: (TBD)
- Maintainer: kittiu (Ecosoft)
