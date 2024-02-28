**Meeting.java**
<p><code>
        @Entity
        public class Meeting{
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Integer meetingId;
        }
</code></p>

**MeetingDTO.java**
<p><code>
public class MeetingDTO {
        private integer meetingId;

        @NotNull(message ="{meeting.schedulername.notpresnet}")
        @Pattern(regexp ="[A-Za-z]+[\s][A-Za-z]+", message ="{meeting.schedulername.invallid}")
        private String schedulerName;

        @NotNull(message ="{meeting.teamname.notpresent}")
        private String teamName;

        @NotNull(message ="meeting.purpose.notpresent}")
        private String purpose;

        @NotNull(message="{meeting.meetingdate.notpresent}")
        @FutureOrPresent(message ="{meeting.meetingdate.invalid}")
        private LocalDate meetingDate;
}        
</code></p>

**MeetingValidator.java**
<p><code>
        public static void validateMeeting(MeetingDTO meetingDTO) throws MeetingSchedulerException {
                if(Boolean.FALSE.equals(isValidTeamName(meetingDTO.getTeamName()))) {
                	throw new MeetingSchedulerException("MeetingValidator.INVALID_TEAM_NAME");
		 }
   		if(!MeetingValidator.isValidMeetingDate(meetingDTO.getMeetingDate())){
     			throw new MeetingSchedulerException("MeetingValidator.INVALID_MEETING_DATE");
		}
  	}
   	public static Boolean isValidTeamName(String teamName) {
    		return teamName.matches("(ETAMYSJAVA|ETAMYSUI|ETAMYSMS|ETAMYSAI|ETAMYSBI)");
      	}

 	public static Boolean isValidMeetingDate(LocalDate seeingDate){
  		DayOfWeek s = meetingDate.getDayOfWeek();
    		if(s.getValue()==1 || s.getValue()==2 || s.getValue()==3 || s.getValue()==4 || s.getValue()==5){
      			return true;
	 	}
   		return false;
     }        
</code></p>

**MeetingRepository.java**
<p><code>
	public interface MeetingRepository extends CrudRepository<*Meeting, Integer>
	{
		List<*Meeting> findBySchedulerName(String schedulerName);
		List<*Meeting> findByMeetingDate(LocalDate meetingDate);
		List<*Meeting> findByTeamName(String teamName);
	}
</code></p>

**MeetingServiceImpl.java**
<p><code>
	@Service(value="meetingService")
	@Transactional
	public class MeetingServiceServiceImpl implements MeetingService{
		@Autowired
		private MeetingRepository meetingRepository;

 		@Override
   		public List<*MeetingDTO> getAllMeetingsOfScheduler(String schedulerName) throws MeetingSchedulerException {
     			List<*Meeting> l =meetingRepository.findBySchedulerName(schedulerName);
			if(l.isEmpty()) {
   				throw new MeetingSchedulerException("MeetingService.NO_MEETINGS_FOUND");
       			}
	  		List<MeetingDTO> meetingDTO = new ArrayList<>();
     			for(Meeting meeting : l)
			{
   				MeetingDTO m = MeetingSchedulerException("MeetingService.NO_MEETINGS_FOUND");
       				meetingDTO.add(m);
	   		}
      			return meetingDTO;
	 	}
   
</code></p>
