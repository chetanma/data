# data
@PutMapping("/transfercomplaint/{complaintId}")
	public String transferComplaint(@PathVariable int complaintId, @RequestBody Complaint complaint) throws UserException{			
		return deptService.transferComplaint(complaintId, complaint.getDepartment().getDeptId());
	}	
  
  @Override
	public String transferComplaint(int complaintId, int deptId) throws UserException {
		Optional<Complaint> complaint = Optional.ofNullable(complaintRepo.findById(complaintId)).get();		
		if(complaint.isEmpty())
			throw new UserException("No Such Complaint found!");
		else
		{			
			Optional<Department> department = Optional.ofNullable(deptRepo.findById(deptId)).get();			
			if(department.isEmpty())
				throw new UserException("No Such Department found!");
			else{
				department.get().setDeptId(deptId);
				complaint.get().setDepartment(department.get());
				complaintRepo.save(complaint.get());
				return "complaint updated successfully!";	
			}					
		}		
	}	
