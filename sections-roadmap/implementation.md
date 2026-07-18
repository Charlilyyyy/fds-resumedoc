# Implementation

## Purpose

Complete `ResumeService` with section detection and career roadmap, and assemble the full labeled response. This is the final backend analysis source.

Design: [section-detection.md](./section-detection.md), [career-roadmap.md](./career-roadmap.md), [response-assembly.md](./response-assembly.md).

---

## File

```
src/main/java/com/resume/analyzer/service/ResumeService.java
```

Controller unchanged.

---

## Complete `ResumeService.java`

```java
package com.resume.analyzer.service;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

@Service
public class ResumeService {

    public String processResume(MultipartFile file, String role) {
        try (InputStream inputStream = file.getInputStream();
             PDDocument document = PDDocument.load(inputStream)) {

            String text = new PDFTextStripper().getText(document);

            List<String> skills = extractSkills(text);
            int score = calculateScore(skills);
            List<String> suggestions = getSuggestions(skills);

            String jobMatch = matchJobRole(skills, role);
            String atsResult = checkATSScore(text);
            String sectionResult = detectSections(text);
            String jobSuggestions = getJobSuggestions(skills, role);

            return "Skills: " + skills
                 + "\nScore: " + score + "/100"
                 + "\nSuggestions: " + suggestions
                 + "\n\n" + jobMatch
                 + "\n\n" + atsResult
                 + "\n\n" + sectionResult
                 + "\n\n" + jobSuggestions;

        } catch (Exception e) {
            e.printStackTrace();
            return "Error while processing resume ❌";
        }
    }

    private List<String> extractSkills(String text) {
        List<String> skillList = new ArrayList<>();
        String lowerText = text.toLowerCase();
        if (lowerText.contains("java"))       skillList.add("Java");
        if (lowerText.contains("spring"))     skillList.add("Spring Boot");
        if (lowerText.contains("mysql") || lowerText.contains("sql")) skillList.add("SQL");
        if (lowerText.contains("html"))       skillList.add("HTML");
        if (lowerText.contains("css"))        skillList.add("CSS");
        if (lowerText.contains("javascript")) skillList.add("JavaScript");
        return skillList;
    }

    private int calculateScore(List<String> skills) {
        int score = 0;
        if (skills.contains("Java"))        score += 20;
        if (skills.contains("Spring Boot")) score += 20;
        if (skills.contains("SQL"))         score += 20;
        if (skills.contains("HTML"))        score += 10;
        if (skills.contains("CSS"))         score += 10;
        if (skills.contains("JavaScript")) score += 20;
        return score;
    }

    private List<String> getSuggestions(List<String> skills) {
        List<String> suggestions = new ArrayList<>();
        if (!skills.contains("Java"))
            suggestions.add("Build strong foundation in Java with OOP concepts");
        if (!skills.contains("Spring Boot"))
            suggestions.add("Create a REST API project using Spring Boot");
        if (!skills.contains("SQL"))
            suggestions.add("Practice database queries and learn MySQL deeply");
        if (!skills.contains("JavaScript"))
            suggestions.add("Work on frontend using JavaScript and build projects");
        if (!skills.contains("HTML") || !skills.contains("CSS"))
            suggestions.add("Improve frontend skills using HTML, CSS and responsive design");
        if (skills.size() < 3)
            suggestions.add("Add at least 2 real-world projects to strengthen your resume");
        return suggestions;
    }

    private Map<String, List<String>> jobRoles() {
        Map<String, List<String>> roles = new HashMap<>();
        roles.put("Java Developer", Arrays.asList("Java", "Spring Boot", "SQL"));
        roles.put("Full Stack Developer", Arrays.asList("Java", "JavaScript", "HTML", "CSS"));
        roles.put("Data Analyst", Arrays.asList("SQL", "Python", "Excel"));
        return roles;
    }

    private String matchJobRole(List<String> skills, String role) {
        List<String> requiredSkills = jobRoles().get(role);
        int matchCount = 0;
        List<String> missing = new ArrayList<>();
        for (String req : requiredSkills) {
            if (skills.contains(req)) matchCount++;
            else missing.add(req);
        }
        int matchScore = (matchCount * 100) / requiredSkills.size();
        return "Role: " + role
             + "\nMatch Score: " + matchScore + "%"
             + "\nMissing Skills: " + missing;
    }

    private List<String> getATSKeywords() {
        return Arrays.asList(
            "java", "spring", "spring boot", "rest api", "microservices",
            "mysql", "sql", "hibernate", "docker", "kubernetes",
            "aws", "javascript", "html", "css");
    }

    private String checkATSScore(String text) {
        List<String> keywords = getATSKeywords();
        List<String> missing = new ArrayList<>();
        String lowerText = text.toLowerCase();
        int matchCount = 0;
        for (String key : keywords) {
            if (lowerText.contains(key)) matchCount++;
            else missing.add(key);
        }
        int score = (matchCount * 100) / keywords.size();
        return "ATS Score: " + score + "%"
             + "\nMissing Keywords: " + missing;
    }

    private String detectSections(String text) {
        String lowerText = text.toLowerCase();
        boolean hasSkills = lowerText.contains("skills");
        boolean hasEducation = lowerText.contains("education");
        boolean hasExperience = lowerText.contains("experience");
        String result = "Resume Sections:\n";
        result += "Skills Section: " + (hasSkills ? "Found ✅" : "Missing ❌") + "\n";
        result += "Education Section: " + (hasEducation ? "Found ✅" : "Missing ❌") + "\n";
        result += "Experience Section: " + (hasExperience ? "Found ✅" : "Missing ❌");
        return result;
    }

    private String getJobSuggestions(List<String> skills, String role) {
        List<String> suggestions = new ArrayList<>();
        if (role.equalsIgnoreCase("Java Developer")) {
            if (!skills.contains("Spring Boot"))
                suggestions.add("Learn Spring Boot and build REST APIs");
            if (!skills.contains("SQL"))
                suggestions.add("Practice MySQL and database design");
            if (!skills.contains("Java"))
                suggestions.add("Strengthen Java fundamentals and OOP concepts");
            suggestions.add("Build a full backend project using Spring Boot");
        } else if (role.equalsIgnoreCase("Full Stack Developer")) {
            if (!skills.contains("JavaScript"))
                suggestions.add("Learn JavaScript and build dynamic web apps");
            if (!skills.contains("HTML") || !skills.contains("CSS"))
                suggestions.add("Improve frontend skills using HTML, CSS");
            suggestions.add("Build a full stack project (frontend + backend)");
        } else if (role.equalsIgnoreCase("Data Analyst")) {
            suggestions.add("Learn Python and data analysis libraries");
            suggestions.add("Practice SQL queries and data visualization");
            suggestions.add("Work on real datasets and dashboards");
        }
        return "Career Roadmap for " + role + ":\n" + suggestions;
    }
}
```

---

## Build and test

```bash
./mvnw clean compile
./mvnw spring-boot:run
```

Upload a PDF + role and confirm the full labeled body (see [response-assembly.md](./response-assembly.md)).

---

## Checklist

- [ ] `detectSections` and `getJobSuggestions` present
- [ ] All blocks assembled in contract order
- [ ] `\n\n` separators between blocks
- [ ] Full pipeline compiles and returns complete body
- [ ] Controller unchanged

---

## Link to prior context

Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — full backend acceptance and close.
