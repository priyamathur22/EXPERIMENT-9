# Deploy Full Stack App on AWS with Load Balancing

This sample repo contains a minimal **React frontend** (build served by Nginx) and an **Express backend**. 
It also includes Dockerfiles and a `docker-compose.yml` to test locally.

## Local testing
- Build and run with Docker Compose:
```bash
docker-compose up --build
```
- Frontend: http://localhost:8080
- Backend: http://localhost:3000 and health at http://localhost:3000/api/health

## Suggested AWS deployment approach (manual steps)
1. Build Docker images for frontend and backend and push them to **Amazon ECR** (or Docker Hub).
2. Launch two or more **EC2 instances** (as target group instances) and install Docker on them.
3. Pull the backend image and run backend containers on each EC2 instance (ensure port 3000 open in Security Group).
4. For the frontend, either serve via S3+CloudFront or run frontend containers on EC2 too.
5. Create an **Application Load Balancer (ALB)**:
   - Create Target Groups for backend (HTTP port 3000) and frontend (HTTP port 80) if you run both on instances.
   - Register EC2 instances with the target groups (same instances can run both services on different ports).
   - Create Listeners and Rules to route paths: e.g. `/api/*` -> backend target group, `/*` -> frontend target group.
6. Configure Security Groups: ALB security group allows inbound 80 (or 443), EC2 security group allows inbound from ALB only.
7. (Optional) Configure Route 53 and SSL certificates via ACM for HTTPS.
8. Test by accessing ALB DNS name in browser.

## Automated options
- Use **ECS/Fargate** with ALB for container orchestration (recommended):
  - Create ECS cluster, task definitions for frontend and backend, and services with desired count >1.
  - Create target groups and attach to services; ALB routes to ECS services.
- Or use **Elastic Beanstalk** or **EKS** depending on needs.

## Notes on health & scaling
- Ensure backend has a health endpoint (e.g., `/api/health`) for ALB target health checks.
- Run at least 2 instances (or desired task count) for high availability.
- Use Auto Scaling groups or ECS service autoscaling to scale with load.

## Extra: Example health endpoint call
```
curl http://<instance-ip>:3000/api/health
```

