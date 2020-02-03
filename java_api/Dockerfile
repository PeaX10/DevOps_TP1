FROM openjdk:11 AS jdk
ADD Main.java /
RUN javac Main.java


FROM openjdk:11-jre AS jre
# Copy resource from previous stage
COPY --from=jdk /Main.class .
# Run java code with the JRE
RUN java Main