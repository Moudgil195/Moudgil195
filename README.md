## Hi, I'm Karan 👋

**Software Engineer · .NET / ASP.NET Core · Backend & APIs**

I build backend systems in C# and .NET: Web APIs, SQL Server, and the architecture that keeps them simple to change. Day job: ~4 years building .NET services, currently in travel tech.

### 🔨 Building now: the ERP for Glass Link Enterprise
Inventory, billing, credit (*udhaar*) and accounting for **Glass Link Enterprise**, my family's glass-hardware wholesale business in Kaithal, Haryana. Built so other wholesalers can use it too.

- **Architecture:** modular monolith (.NET 10, Minimal APIs, vertical slices), module boundaries enforced by architecture tests
- **Data:** EF Core + SQL Server, multi-tenant via global query filters, append-only audit trail, Guid v7 keys
- **Security:** HttpOnly SameSite cookies, CSRF header guard, lockout + rate limiting, security-stamp session revocation
- **Domain:** GST-aware (works before *and* after GST registration), Indian money formatting, GSTIN checksum validation
- **Frontend:** React + TypeScript (Vite, Mantine, TanStack Query), Hindi/English, installable on phones
- **Quality:** unit, architecture and real-SQL integration tests

*The repository is private (it's a product). Happy to walk through the code and design in an interview.*

### 🧰 Tech
`C#` `.NET 10` `ASP.NET Core` `EF Core` `SQL Server` `REST` `React` `TypeScript` `Angular` `xUnit` `Git` `IIS / Windows hosting`

### 📌 Earlier work
- **PKS Sentinel**: multi-tenant hospital/clinic platform (appointments, queue management, lab, pharmacy). Paused.
- **MoRTH (Govt. of India) project**: ASP.NET MVC web app, and a .NET Core Web API for its mobile version.

### 📫 Contact
[LinkedIn](https://www.linkedin.com/in/karansharmakaran)
