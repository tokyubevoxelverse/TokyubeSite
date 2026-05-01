# Tokyube Bug Reporting System Setup Guide

## ✅ What's Been Added

Your website now has:
- **Lilac "FILE A BUG" button** at the bottom of the sidebar menu (visible when hovering on the right edge)
- **Bug report form modal** with fields for:
  - Bug Title (required)
  - Description (required)
  - Severity level (Low/Medium/High/Critical)
  - Steps to reproduce
  - Email (optional - for follow-up)
- **Beautiful styling** matching your Tokyube aesthetic

---

## 🚀 FREE BUG REPORTING SOLUTIONS

### **Option 1: Formspree (RECOMMENDED - Easiest)**

Formspree is completely free for up to **50 submissions/month** and sends all bug reports directly to your email.

#### Setup Steps:

1. **Visit:** https://formspree.io
2. **Sign up** with your GitHub account or email
3. **Create a new form:**
   - Project name: `Tokyube Bug Reports`
   - Email: Your email address where you want to receive bug reports
4. **Copy your Form ID** - it will look like: `mbjqxqln` or similar
5. **Update `index.html`:**
   - Find this line in the JavaScript: `https://formspree.io/f/YOUR_FORM_ID`
   - Replace `YOUR_FORM_ID` with the ID you got from Formspree
   - Example: `https://formspree.io/f/mbjqxqln`

**That's it!** Your bug reports will now email you directly.

---

### **Option 2: GitHub Issues API (Even Better - Direct to Your Repo)**

If you want bug reports to automatically create issues in your GitHub repo:

1. **Create a GitHub Personal Access Token:**
   - Go to: https://github.com/settings/tokens
   - Click "Generate new token"
   - Select scopes: `repo` (full access)
   - Copy the token

2. **You'll need a backend** to securely handle this (Node.js, Python, etc.)
   - The JavaScript can't directly use GitHub tokens (security risk)
   - Option 2B below shows an alternative

---

### **Option 2B: GitHub Issues via Formspree**

You can use Formspree AND have it send a custom webhook to GitHub:

1. Set up Formspree as in **Option 1**
2. In Formspree dashboard, go to **Integrations**
3. Add a **Webhook** pointing to a GitHub Actions workflow
4. This creates GitHub issues automatically from bug reports

---

### **Option 3: Discord Webhook (FREE - Get Real-Time Alerts)**

If you want instant notifications in Discord:

1. **Create a Discord server** (if you don't have one)
2. **Create a webhook:**
   - Right-click channel → Edit channel
   - Integrations → Webhooks → New Webhook
   - Copy the webhook URL
3. **Update the form handler** to send to Discord instead of email

---

## 📋 Current Setup

The form is currently configured to use **Formspree**. Just follow the setup above to connect it!

### The form collects:
```javascript
{
  title: "Bug title",
  description: "Detailed description",
  severity: "Low/Medium/High/Critical",
  steps: "Steps to reproduce",
  email: "user@email.com" // optional
}
```

---

## 🔧 How It Works

When users click the lilac "FILE A BUG" button:
1. A beautiful modal form appears
2. They fill in bug details
3. They click "Submit Report"
4. The form data is sent securely to your chosen service
5. You receive the report (via email, Discord, or GitHub)

---

## 🛠️ Alternative: Discord Webhook Implementation

If you want to use Discord instead, here's the updated JavaScript snippet:

```javascript
// Replace the form submission handler with this:
bugReportForm.addEventListener('submit', async (e) => {
  e.preventDefault();
  submitBugBtn.disabled = true;
  formStatus.textContent = 'Sending...';
  formStatus.className = 'form-status';

  const formData = {
    content: `🐛 **New Bug Report**\n\n**Title:** ${document.getElementById('bugTitle').value}\n**Severity:** ${document.getElementById('bugSeverity').value}\n**Description:** ${document.getElementById('bugDescription').value}\n**Steps:** ${document.getElementById('bugSteps').value}\n**Email:** ${document.getElementById('bugEmail').value || 'Not provided'}`
  };

  try {
    const response = await fetch('YOUR_DISCORD_WEBHOOK_URL', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData)
    });

    if (response.ok) {
      formStatus.textContent = '✓ Bug report submitted! Thank you!';
      formStatus.className = 'form-status success';
      bugReportForm.reset();
      setTimeout(closeReportModal, 3000);
    }
  } catch (error) {
    formStatus.textContent = '✗ Error submitting report.';
    formStatus.className = 'form-status error';
    submitBugBtn.disabled = false;
  }
});
```

---

## 💡 Recommendation

**Use Formspree** - it's:
- ✅ Free (50 reports/month)
- ✅ Super easy to set up (2 minutes)
- ✅ Reports go to your email
- ✅ No code changes needed beyond pasting one ID
- ✅ Can integrate with other services
- ✅ GDPR compliant

---

## 📞 Need Help?

- **Formspree Docs:** https://formspree.io/help/
- **GitHub Pages & Forms:** GitHub Pages is static only, so you need a backend service like Formspree
- **Discord Webhooks:** https://discord.com/developers/docs/resources/webhook

---

**Next Step:** Get your Formspree Form ID and update line in the JavaScript! 🎉
