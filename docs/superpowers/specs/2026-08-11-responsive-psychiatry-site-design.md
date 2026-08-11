# Renaissance Psychiatry Responsive Website Design

## Objective

Convert the supplied self-unpacking website export into a clean, maintainable, standalone HTML page. Preserve the existing Renaissance Psychiatry visual identity and content while making the page comfortable to use on phones, tablets, laptops, and wide desktop screens. Replace image placeholders with suitable, stable Unsplash photography.

## Deliverable

Create `renaissance-psychiatry-responsive.html` in the project root. The page will contain its semantic HTML, CSS, and lightweight JavaScript in one file so it can be opened directly in a browser or uploaded to a simple static host.

The original file in Downloads will remain unchanged.

## Visual Direction

- Preserve the navy, blue, aqua, white, and pale-blue palette from the supplied design.
- Preserve the Plus Jakarta Sans typographic character, with a system-font fallback if the web font cannot load.
- Retain the rounded cards, generous spacing, strong headings, subtle shadows, and calm clinical tone.
- Use restrained animation only for hover, menu, tab, and accordion feedback. Respect `prefers-reduced-motion`.
- Use specific Unsplash image URLs rather than randomized search URLs so the selected photography remains stable.

## Page Structure

The responsive page will include:

1. New-patient notice bar.
2. Sticky header with brand, desktop navigation, patient portal action, booking action, and an accessible mobile menu.
3. Hero with headline, supporting copy, calls to action, care statistics, clinician/patient image, and continuity-of-care callout.
4. In-person and telehealth comparison with working tabs.
5. Four service cards.
6. Conditions treated.
7. Four age-group cards with matching Unsplash photography for children/family, adolescents, adults, and older adults.
8. Provider-directory introduction.
9. Three-step new-patient process and payment information.
10. Accessible FAQ accordion.
11. Appointment call-to-action and illustrative booking interface.
12. Footer with crisis guidance, navigation, office details, and required informational disclaimer.

## Responsive Behavior

- Constrain content to a readable maximum width while allowing full-width section backgrounds.
- Use fluid type and spacing with `clamp()` where helpful.
- At tablet widths, reduce multi-column grids to two columns and stack large split layouts when space becomes tight.
- Below approximately 900px, replace desktop navigation/actions with a menu button and collapsible menu.
- At phone widths, use single-column sections, full-width primary actions, smaller page gutters, compact cards, and repositioned or simplified decorative overlays.
- Prevent long email addresses, labels, and booking rows from causing horizontal overflow.
- Maintain touch targets of roughly 44px or larger.

## Images

- Select professional, calm, inclusive healthcare or conversational imagery from Unsplash.
- Avoid alarming clinical imagery and avoid photographs that imply a real staff member is employed by the practice.
- Provide descriptive alt text for meaningful photographs.
- Use `loading="lazy"` and `decoding="async"` below the fold; prioritize the hero image.
- Apply consistent crops with `object-fit: cover` and responsive aspect ratios.
- Provide a neutral background color so layouts remain coherent while images load or if a remote image fails.

## Interaction and Accessibility

- Use semantic landmarks and heading order.
- Give the mobile navigation button `aria-expanded` and an associated menu label.
- Make care tabs keyboard operable with tab semantics and correctly associated panels.
- Implement FAQs as buttons with `aria-expanded` and controlled answer regions.
- Close mobile navigation after a menu link is selected and when Escape is pressed.
- Keep visible focus states and sufficient color contrast.
- Do not use JavaScript for layout; core content remains available if JavaScript fails.

## Implementation Boundaries

- This is a front-end-only static page. Booking, portal, telephone, email, address, insurance, and provider details remain placeholders where the supplied design has not provided verified production data.
- No server, CMS, analytics, form submission, or third-party booking integration will be added.
- The existing supplied export will not be edited in place.

## Verification

- Validate that the page loads without local asset dependencies or JavaScript errors.
- Test at representative phone, tablet, laptop, and desktop viewport widths.
- Confirm there is no unintended horizontal scrolling.
- Exercise the mobile navigation, care tabs, FAQ accordion, and in-page links using pointer and keyboard input.
- Confirm every image has useful alternative text and a stable source.
- Inspect rendered desktop and mobile screenshots for clipping, awkward wrapping, spacing problems, and image-crop issues.
