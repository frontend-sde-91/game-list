# game-list
/*
package com.example.bitbucket;

import org.eclipse.jgit.api.Git;
import org.eclipse.jgit.api.errors.GitAPIException;
import org.eclipse.jgit.transport.UsernamePasswordCredentialsProvider;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.io.*;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.UUID;

@RestController
@RequestMapping("/bitbucket")
public class BitbucketController {

    @Value("${bitbucket.repo.url}")
    private String repoUrl;

    @Value("${bitbucket.username}")
    private String username;

    @Value("${bitbucket.token}")
    private String accessToken;

    public static class CodePushRequest {
        public String branchName;
        public String baseBranch;
        public String filePath;
        public String newCode;
        public int insertAfterLine;
    }

    @PostMapping("/push-code")
    public ResponseEntity<String> pushCode(@RequestBody CodePushRequest request) {
        Path tempDir = null;
        try {
            // 1. Create temp directory for clone
            tempDir = Files.createTempDirectory("git-clone-");

            // 2. Clone the repo using access token
            Git git = Git.cloneRepository()
                    .setURI(repoUrl)
                    .setDirectory(tempDir.toFile())
                    .setCredentialsProvider(new UsernamePasswordCredentialsProvider(username, accessToken))
                    .call();

            // 3. Checkout base branch
            git.checkout().setName(request.baseBranch).call();

            // 4. Create and checkout new branch
            git.checkout().setCreateBranch(true).setName(request.branchName).call();

            // 5. Locate file
            File targetFile = new File(tempDir.toFile(), request.filePath);
            if (!targetFile.exists()) {
                return ResponseEntity.badRequest().body("File not found: " + request.filePath);
            }

            // 6. Insert new code
            File tempFile = new File(tempDir.toFile(), UUID.randomUUID() + ".tmp");
            try (BufferedReader reader = new BufferedReader(new FileReader(targetFile));
                 BufferedWriter writer = new BufferedWriter(new FileWriter(tempFile))) {

                String line;
                int currentLine = 1;
                while ((line = reader.readLine()) != null) {
                    writer.write(line);
                    writer.newLine();
                    if (currentLine == request.insertAfterLine) {
                        writer.write(request.newCode);
                        writer.newLine();
                    }
                    currentLine++;
                }
            }

            // 7. Replace original file with updated one
            Files.delete(targetFile.toPath());
            Files.move(tempFile.toPath(), targetFile.toPath());

            // 8. Commit and push
            git.add().addFilepattern(request.filePath).call();
            git.commit().setMessage("Auto-commit from API: " + request.branchName).call();
            git.push()
                    .setCredentialsProvider(new UsernamePasswordCredentialsProvider(username, accessToken))
                    .call();

            return ResponseEntity.ok("✅ Code pushed to branch: " + request.branchName);

        } catch (IOException | GitAPIException e) {
            return ResponseEntity.internalServerError().body("❌ Error: " + e.getMessage());
        } finally {
            if (tempDir != null && tempDir.toFile().exists()) {
                tempDir.toFile().delete(); // Clean up cloned repo folder
            }
        }
    }
}
*/
