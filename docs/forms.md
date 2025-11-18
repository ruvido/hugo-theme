# Custom Forms Documentation

This theme supports custom forms with dynamic fields defined in page YAML frontmatter.

## Setup PocketBase Collection

Create a collection named `forms` in PocketBase with the following schema:

**Collection: `forms`**

| Field | Type | Required | Options |
|-------|------|----------|---------|
| `name` | text | yes | Form identifier (e.g., "feedback-contact") |
| `data` | json | yes | Form field data submitted by user |
| `page_url` | url | no | URL of the page where form was submitted |
| `timestamp` | text | no | ISO timestamp of submission |

**Permissions:**
- Create: Allow public (for form submissions)
- Read: Admin only
- Update: Admin only
- Delete: Admin only

## Creating a Form Page

1. Create a markdown file in `content/forms/` directory
2. Add frontmatter with form configuration:

```yaml
---
title: "Contact Us"
type: forms
form:
  name: "feedback-contact"  # Stored in PocketBase
  submitLabel: "Send Message"  # Optional, default: "Invia"
  successMessage: |
    ✅ **Thank you!**
    Your message has been sent successfully.
  errorMessage: |
    ❌ **Oops!**
    Something went wrong. Please try again later.
  fields:
    - type: "text"
      name: "name"
      label: "Name"
      placeholder: "Your name"
      required: true
    - type: "email"
      name: "email"
      label: "Email"
      placeholder: "your-email@example.com"
      required: true
    - type: "textarea"
      name: "message"
      label: "Message"
      placeholder: "Write your message here..."
      required: true
---

Your page content goes here.
```

## Supported Field Types

- `text` - Single line text input
- `email` - Email input with validation
- `textarea` - Multi-line text area

## Field Options

- `type` (required) - Field type
- `name` (required) - Field identifier (used in JSON data)
- `label` (required) - Label shown to user
- `placeholder` (optional) - Placeholder text
- `required` (optional) - Boolean, default: false

## Form Behavior

- Uses Alpine.js for reactive form handling
- Validates required fields client-side
- Submits to PocketBase `forms` collection
- Shows success/error messages
- Resets form after successful submission
- Includes page URL and timestamp automatically

## Styling

Forms use Pico CSS framework loaded only on pages with `type: forms`.

The theme's existing message styles (`.success`, `.error`) are used for feedback messages.

## Example PocketBase Data Structure

When a form is submitted, the data is stored as:

```json
{
  "name": "feedback-contact",
  "data": {
    "name": "John Doe",
    "email": "john@example.com",
    "message": "Hello, this is a test message."
  },
  "page_url": "https://realmen.it/forms/contact/",
  "timestamp": "2025-01-17T10:30:00.000Z"
}
```

## Example Form Page

See `content/forms/contact.md` for a complete working example.

## Email Notifications (Optional)

To send email notifications when forms are submitted, see PocketBase documentation:
https://pocketbase.io/docs/go-sending-emails/
